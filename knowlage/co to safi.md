
Subsequent Address Family Identifier (SAFI) może przyjmować wiele wartości, które określają typ informacji adresowej przesyłanej w BGP. Oto najważniejsze wartości SAFI wraz z ich opisem:

- 0: Reserved (zarezerwowane)
- 1: Network Layer Reachability Information (NLRI) dla unicast forwarding (adresy unicast)[1][3]
- 2: NLRI dla multicast forwarding (adresy multicast)[1][3]
- 3: Reserved (zarezerwowane)
- 4: NLRI z etykietami MPLS (MPLS Labels)[1]
- 5: Multicast VPN (MCAST-VPN)[1]
- 6: Dynamic Placement of Multi-Segment Pseudowires[1]
- 8: Multicast VPLS (MCAST-VPLS)[1]
- 9: BGP Service Function Chaining (BGP SFC)[1]
- 64: Tunnel SAFI (tunelowanie)[1]
- 65: Virtual Private LAN Service (VPLS)[1]
- 66: BGP MDT SAFI (Multicast Distribution Tree)[1]
- 67: BGP 4over6 SAFI (IPv4 over IPv6 tunneling)[1]
- 68: BGP 6over4 SAFI (IPv6 over IPv4 tunneling)[1]
- 69: Layer-1 VPN auto-discovery[1]
- 70: BGP Ethernet VPN (EVPN)[1]
- 71: BGP Link-State (BGP-LS)[1]
- 72: BGP-LS VPN[1]
- 73: Segment Routing Policy SAFI[1]
- 128: MPLS-labeled VPN address[1]
- 129: Multicast for BGP/MPLS IP VPNs[1]
- 132: Route Target constraints[1]
- 133, 134: Dissemination of Flow Specification rules (standard i L3VPN)[1]
- 140: VPN auto-discovery[1]

Wartości od 10 do 63 oraz 81-82 i 86-127 są nieprzydzielone lub zarezerwowane, a zakres 241-254 jest zarezerwowany do użytku prywatnego[1].

SAFI jest używany razem z AFI (Address Family Identifier) do określania rodzaju tras i informacji wymienianych w BGP, np. unicast IPv4 to AFI=1 i SAFI=1, multicast IPv4 to AFI=1 i SAFI=2[3].

## Podsumowując, SAFI definiuje typ przesyłanych informacji adresowych w BGP i ma wiele zdefiniowanych wartości, które odpowiadają różnym zastosowaniom i rozszerzeniom protokołu BGP[1][3].

Citations:
[1] https://www.iana.org/assignments/safi-namespace
[2] https://datatracker.ietf.org/doc/rfc5512/
[3] https://support.huawei.com/enterprise/en/doc/EDOC1000178171/a32656e4/mp-bgp-and-address-families
[4] https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9400/software/release/16-9/configuration_guide/rtng/b_169_rtng_9400_cg/b_169_rtng_9400_cg_chapter_010000.pdf
[5] https://www.cisco.com/c/en/us/td/docs/ios/12_2sr/12_2srb/feature/guide/srbgpl2v.html
[6] https://www.noction.com/blog/ipv4-bgp-vs-ipv6-bgp
[7] https://docs.frrouting.org/en/latest/bgp.html
[8] https://infocenter.nokia.com/public/7750SR222R1A/topic/com.nokia.Unicast_Guide/multi-protocol_-ai9exj5yje.html

---
Odpowiedź od Perplexity: pplx.ai/share