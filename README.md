# mod_deepweb

## Introduction

This plugin allows Prosody to connect to other servers that are running
as a Tor or I2P  hidden service. Running Prosody on a hidden service works
without this module, this module is only necessary to allow Prosody to
federate to hidden XMPP servers.


For general info about creating a hidden service, see
https://www.torproject.org/docs/tor-hidden-service.html.en.


## Usage

This module depends on the bit32 Lua library.

To create a hidden service that can federate with other hidden XMPP
servers, first add a hidden serivce to Tor. It should listen on port
5269 and optionally also on 5222 (if c2s connections to the hidden
service should be allowed).

Use the hostname that Tor gives with a virtualhost:

```
VirtualHost "555abcdefhijklmn.onion"
    modules_enabled = { "deepweb" };
```

Enable it under a clearnet host to allow federation with hidden servers:

```
VirtualHost "xmpp.example.com"
    modules_enabled = { "deepweb" };
```

## Configuration


Name |	Description 	| Type 	| Default value  
--- | --- | --- | ---
onion\_socks5_host |	the host to connect to for Tor’s SOCKS5 proxy 	| string 	| “127.0.0.1”
onion\_socks5_port |	the port to connect to for Tor’s SOCKS5 proxy 	| integer 	| 9050
i2p\_socks5_host |	the host to connect to for I2P’s SOCKS5 proxy 	| string 	| “127.0.0.1”
i2p\_socks5_port |	the port to connect to for I2P’s SOCKS5 proxy 	| integer 	| 4447
deepweb_only 	| forbid all connection attempts to non-"deepweb" servers |	boolean 	| false
deepweb\_tor_all 	| pass all s2s connections through Tor 	| boolean 	| false
onion_map 	| override the address for a host through onion	| table 	| {}
i2p_map 	| override the address for a  host through i2p| table 	| {}


By setting `onion_map` or `i2p_map`, it is possible to override the address used to
connect to a given host with the address of a hidden service. The
configuration works as follows:

```
 onions_map = {
        ["kalli.st"] = "kallist4mcluuxbjnr5p2asdlmdhaos3pcrvhk3fbzmiiiftwg6zncid.onion";
}
```

or, to also specify a port:


```
 onions_map = {
        ["kalli.st"] = { 
        	host = "kallist4mcluuxbjnr5p2asdlmdhaos3pcrvhk3fbzmiiiftwg6zncid.onion",  port= 5269
        };
}
```