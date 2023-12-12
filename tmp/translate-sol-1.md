
### Back to mqtt module



The PUBLISH packet now:

{% highlight markdown %}

 |   Bit    |  7  |  6  |  5  |  4  |  3  |  2  |  1  |   0    |  <-- Fixed Header
 |----------|-----------------------|--------------------------|
 | Byte 1   |      MQTT type 3      | dup |    QoS    | retain |
 |----------|--------------------------------------------------|
 | Byte 2   |                                                  |
 |  .       |               Remaining Length                   |
 |  .       |                                                  |
 | Byte 5   |                                                  |
 |----------|--------------------------------------------------|  <-- Variable Header
 | Byte 6   |                Topic len MSB                     |
 | Byte 7   |                Topic len LSB                     |
 |-------------------------------------------------------------|
 | Byte 8   |                                                  |
 |   .      |                Topic name                        |
 | Byte N   |                                                  |
 |----------|--------------------------------------------------|
 | Byte N+1 |            Packet Identifier MSB                 |
 | Byte N+2 |            Packet Identifier LSB                 |
 |----------|--------------------------------------------------|  <-- Payload
 | Byte N+3 |                   Payload                        |
 | Byte N+M |                                                  |

{% endhighlight %}


Packet identifier MSB and LSB are present in the packet if and only if the QoS
level is > 0, with a QoS set to *at most once* there's no need for a packet ID.
Payload length is calculated by subtracting the Remaining Length with all the
other fields already unpacked.

<hr>
**src/mqtt.c**
<hr>

{% highlight c %}

static size_t unpack_mqtt_publish(const unsigned char *buf,
                                  union mqtt_header *hdr,
                                  union mqtt_packet *pkt) {
    struct mqtt_publish publish = { .header = *hdr };
    pkt->publish = publish;
    /*
     * Second byte of the fixed header, contains the length of remaining bytes
     * of the connect packet
     */
    size_t len = mqtt_decode_length(&buf);
    /* Read topic length and topic of the soon-to-be-published message */
    pkt->publish.topiclen = unpack_string16(&buf, &pkt->publish.topic);
    uint16_t message_len = len;
    /* Read packet id */
    if (publish.header.bits.qos > AT_MOST_ONCE) {
        pkt->publish.pkt_id = unpack_u16((const uint8_t **) &buf);
        message_len -= sizeof(uint16_t);
    }
    /*
     * Message len is calculated subtracting the length of the variable header
     * from the Remaining Length field that is in the Fixed Header
     */
    message_len -= (sizeof(uint16_t) + topic_len);
    pkt->publish.payloadlen = message_len;
    pkt->publish.payload = malloc(message_len + 1);
    unpack_bytes((const uint8_t **) &buf, message_len, pkt->publish.payload);
    return len;
}

{% endhighlight %}
<hr>

Subscribe and unsubscribe packets are fairly similar, they reflect the
PUBLISH packet, but for payload they have a list of tuple consisting in a
pair (topic, QoS). Their implementation is practically identical, with the
only difference in the payload part, where UNSUBSCRIBE doesn't specify QoS
for each topic.

<hr>
**src/mqtt.c**
<hr>

{% highlight c %}

static size_t unpack_mqtt_subscribe(const unsigned char *buf,
                                    union mqtt_header *hdr,
                                    union mqtt_packet *pkt) {
    struct mqtt_subscribe subscribe = { .header = *hdr };
    /*
     * Second byte of the fixed header, contains the length of remaining bytes
     * of the connect packet
     */
    size_t len = mqtt_decode_length(&buf);
    size_t remaining_bytes = len;
    /* Read packet id */
    subscribe.pkt_id = unpack_u16((const uint8_t **) &buf);
    remaining_bytes -= sizeof(uint16_t);
    /*
     * Read in a loop all remaining bytes specified by len of the Fixed Header.
     * From now on the payload consists of 3-tuples formed by:
     *  - topic length
     *  - topic filter (string)
     *  - qos
     */
    int i = 0;
    while (remaining_bytes > 0) {
        /* Read length bytes of the first topic filter */
        remaining_bytes -= sizeof(uint16_t);
        /* We have to make room for additional incoming tuples */
        subscribe.tuples = realloc(subscribe.tuples,
                                   (i+1) * sizeof(*subscribe.tuples));
        subscribe.tuples[i].topic_len =
            unpack_string16(&buf, &subscribe.tuples[i].topic);
        remaining_bytes -= subscribe.tuples[i].topic_len;
        subscribe.tuples[i].qos = unpack_u8((const uint8_t **) &buf);
        len -= sizeof(uint8_t);
        i++;
    }
    subscribe.tuples_len = i;
    pkt->subscribe = subscribe;
    return len;
}

static size_t unpack_mqtt_unsubscribe(const unsigned char *buf,
                                      union mqtt_header *hdr,
                                      union mqtt_packet *pkt) {
    struct mqtt_unsubscribe unsubscribe = { .header = *hdr };
    /*
     * Second byte of the fixed header, contains the length of remaining bytes
     * of the connect packet
     */
    size_t len = mqtt_decode_length(&buf);
    size_t remaining_bytes = len;
    /* Read packet id */
    unsubscribe.pkt_id = unpack_u16((const uint8_t **) &buf);
    remaining_bytes -= sizeof(uint16_t);
    /*
     * Read in a loop all remaining bytes specified by len of the Fixed Header.
     * From now on the payload consists of 2-tuples formed by:
     *  - topic length
     *  - topic filter (string)
     */
    int i = 0;
    while (remaining_bytes > 0) {
        /* Read length bytes of the first topic filter */
        remaining_bytes -= sizeof(uint16_t);
        /* We have to make room for additional incoming tuples */
        unsubscribe.tuples = realloc(unsubscribe.tuples,
                                     (i+1) * sizeof(*unsubscribe.tuples));
        unsubscribe.tuples[i].topic_len =
            unpack_string16(&buf, &unsubscribe.tuples[i].topic);
        remaining_bytes -= unsubscribe.tuples[i].topic_len;
        i++;
    }
    unsubscribe.tuples_len = i;
    pkt->unsubscribe = unsubscribe;
    return len;
}

{% endhighlight %}
<hr>

And finally the ACK. In MQTT doesn't exists generic acks, but the structure
of most of the ack-like packets is practically the same for each one, formed by
a header and a packet ID.

These are:

- PUBACK
- PUBREC
- PUBREL
- PUBCOMP
- UNSUBACK

<hr>
**src/mqtt.c**
<hr>

{% highlight c %}

static size_t unpack_mqtt_ack(const unsigned char *buf,
                              union mqtt_header *hdr,
                              union mqtt_packet *pkt) {
    struct mqtt_ack ack = { .header = *hdr };
    /*
     * Second byte of the fixed header, contains the length of remaining bytes
     * of the connect packet
     */
    size_t len = mqtt_decode_length(&buf);
    ack.pkt_id = unpack_u16((const uint8_t **) &buf);
    pkt->ack = ack;
    return len;
}

{% endhighlight %}
<hr>

We have now all needed helpers functions to implement our only exposed function
on the header `mqtt_mqtt_packet`. It ended up being a fairly short and simple
function, we'll use a static array to map all helper functions, making it O(1)
the selection of the correct unpack function based on the Control Packet type.
To be noted that in case of a disconnect, a pingreq or pingresp packet we only
need a single byte, with remaining length 0.

<hr>
**src/mqtt.c**
<hr>

{% highlight c %}

typedef size_t mqtt_unpack_handler(const unsigned char *,
                                   union mqtt_header *,
                                   union mqtt_packet *);
/*
 * Unpack functions mapping unpacking_handlers positioned in the array based
 * on message type
 */
static mqtt_unpack_handler *unpack_handlers[11] = {
    NULL,
    unpack_mqtt_connect,
    NULL,
    unpack_mqtt_publish,
    unpack_mqtt_ack,
    unpack_mqtt_ack,
    unpack_mqtt_ack,
    unpack_mqtt_ack,
    unpack_mqtt_subscribe,
    NULL,
    unpack_mqtt_unsubscribe
};

int unpack_mqtt_packet(const unsigned char *buf, union mqtt_packet *pkt) {
    int rc = 0;
    /* Read first byte of the fixed header */
    unsigned char type = *buf;
    union mqtt_header header = {
        .byte = type
    };
    if (header.bits.type == DISCONNECT
        || header.bits.type == PINGREQ
        || header.bits.type == PINGRESP)
        pkt->header = header;
    else
        /* Call the appropriate unpack handler based on the message type */
        rc = unpack_handlers[header.bits.type](++buf, &header, pkt);
    return rc;
}

{% endhighlight %}
<hr>

The first part ends here, at this point we have two modules, one of utility for
general serialization operations and one to handle the protocol itself accordingly
to the standard defined by OASIS.

{% highlight bash %}
sol/
 ├── src/
 │    ├── mqtt.h
 │    ├── mqtt.c
 │    ├── pack.h
 │    └── pack.c
 ├── CHANGELOG
 ├── CMakeLists.txt
 ├── COPYING
 └── README.md
{% endhighlight %}

Just `git commit` and `git push`. Cya.

[Part-2](../sol-mqtt-broker-p2) will deal with the
networking utilities needed to setup our communication layer and thus the server.