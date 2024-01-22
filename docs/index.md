---
Title: Homeautomation Documentation
---

<a href="https://www.buymeacoffee.com/rangulvers" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

Collection of scripts, tools, hardware and other elements used for our home automation setup. Everything is controlled by my [home assistant](https://www.home-assistant.io/) installation. The following parts will focus on this setup.

### Support

[community.home.assistant.io](https://community.home-assistant.io/)

[Discord](https://discord.com/invite/c5DvZ4e)

```mermaid
    flowchart TB
        subgraph Network
            rout1(Router) ---> net1
        end
        subgraph AP
            net1(Switch) ---> net2(Access Point)
            net1(Switch) ---> net3(Access Point)
            net1(Switch) ---> net4(Access Point)
        end
        subgraph WiFi
            net2 -...- wifi1(Clients)
            net3 -...- wifi1(Clients)
            net4 -...- wifi1(Clients)
        end
        subgraph Cameras
            net1{Switch} ---> cam1(Cameras) 
        end
        subgraph Storage
            net1 ---> stor1(QNAP)
            cam1 -...-> stor1
        end
        subgraph RaspberryPI
            ha1(Home Assistant) ---> ha2(ConBeeII)
            net1 ---> ha1
        end
        subgraph ZigBee
            ha2 -...- zb1c(ZigBee Clients)
        end

```