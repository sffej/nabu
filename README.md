# Nabu

A minimal Java implementation of [IPFS](https://ipfs.io)

[Nabu](https://en.wikipedia.org/wiki/Nabu) is the ancient Mesopotamian patron god of literacy, the rational arts, scribes, and wisdom.

## Status
This is a WIP. You can follow our progress updates [here](https://peergos.net/public/ianopolous/work/java-ipfs-updates.md?open=true).

Currently implemented properties:
* TCP transport
* Noise encryption and authentication
* TLS security provider (with early muxer negotiation using ALPN)
* RSA and Ed25519 peer IDs
* yamux and mplex muxers
* Kademlia DHT for peer routing, IPNS publishing and fallback content discovery
* Bitswap 1.2.0 + auth extension as [used in Peergos](https://peergos.org/posts/bats)
* p2p http proxy
* dnsaddr multiaddr resolution during bootstrap
* autonat
* uPnP port forwarding
* nat-pmp port forwarding
* file and S3 based blockstore
* persistent datastores (IPNS record store) using H2 DB
* persistent identities and config
* basic HTTP API (block.{get, put, rm, has, stat}, id, getRefs, bloomAdd) compatible with matching kubo api calls
* bloom/[infini filtered](https://www.rasmuspagh.net/papers/infinifilter.pdf) blockstore
* connect bitswap to kademlia for discovery, with a faster version with supplied peerids
* configurable cid publishing function

In the future we will add:
* circuit-relay
* dcutr (direct connection upgrade through relay)
* AutoRelay
* mDNS peer discovery
* Android compatibility
* example serverless chat app using p2p http proxy for Android and iOS
* QUIC transport (and encryption and multiplexing)

## Usage
You can use this as a standalone application for storing and retrieving blocks. Or you can embed it in your application. 

### Maven, Gradle, SBT

Package managers are supported through [JitPack](https://jitpack.io/#Peergos/nabu) which supports Maven, Gradle, SBT, etc.

for Maven, add the following sections to your pom.xml (replacing $LATEST_VERSION):
```
  <repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
  </repositories>

  <dependencies>
    <dependency>
      <groupId>com.github.peergos</groupId>
      <artifactId>nabu</artifactId>
      <version>$LATEST_VERSION</version>
    </dependency>
  </dependencies>
```


### S3 Blockstore

By default, Nabu will construct a File based blockstore

If you are configuring a brand new Nabu instance without any data, you can enable S3 by passing in a command line parameter:
```
-s3.datastore "{\"region\": \"us-east-1\", \"bucket\": \"$bucketname\", \"rootDirectory\": \"$bucketsubdirectory\", \"regionEndpoint\": \"us-east-1.linodeobjects.com\", \"accessKey\": \"1\", \"secretKey\": \"2\"}
```
Note: accessKey and secretKey are optional. They can be set via env vars AWS_ACCESS_KEY_ID & AWS_SECRET_ACCESS_KEY or read from ~/.aws/credentials
