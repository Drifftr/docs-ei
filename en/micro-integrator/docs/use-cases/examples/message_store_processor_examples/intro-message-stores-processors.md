# Introduction to Message Store

## Example use case

This sample demonstrates the basic functionality of a [message
store](https://docs.wso2.com/display/EI650/Message+Stores) .

## Synapse configuration

The XML configuration for this sample is as follows:

```xml tab='Fault Sequence'
<sequence name="fault">
    <log level="full">
        <property name="MESSAGE" value="Executing default 'fault' sequence"/>
        <property name="ERROR_CODE" expression="get-property('ERROR_CODE')"/>
        <property name="ERROR_MESSAGE" expression="get-property('ERROR_MESSAGE')"/>
    </log>
    <drop/>
</sequence>
```

```xml tab='On Store Sequence'
<sequence name="onStoreSequence">
    <log>
        <property name="On-Store" value="Storing message"/>
    </log>
</sequence>
```

```xml tab='Main Sequence'
<sequence name="main">
    <in>
        <log level="full"/>
        <property name="FORCE_SC_ACCEPTED" value="true" scope="axis2"/>
        <store messageStore="MyStore" sequence="onStoreSequence"/>
    </in>
    <description>The main sequence for the message mediation</description>
</sequence>
```

## Build and run

When you execute the client you will see that the message is dispatched
to the main sequence.

In the main sequence, the store mediator will store the
`         placeorder        ` request message in the
`         MyStore        ` message store. Before storing the request,
the message store mediator will invoke the
`         onStoreSequence        ` sequence.

Analyze the output debug messages and you will see the following log:

```xml
INFO - LogMediator To: http://localhost:9000/services/SimpleStockQuoteService, WSAction: urn:placeOrder, SOAPAction: urn:placeOrder, ReplyTo: http://www.w3.org/2005/08/addressing/none, MessageID: urn:uuid:54f0e7c6-7b43-437c-837e-a825d819688c, Direction: request, On-Store = Storing message
```

You can then use the JMX view of the Synapse message store by using the
jconsole in order to view the stored messages and delete them.
