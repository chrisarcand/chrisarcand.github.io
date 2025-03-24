---
layout: post
title: "Zero Trust: Securely Accessing Home Assistant with Cloudflare Tunnels"
tags: [Tech]
description: "Implement Zero Trust architecture in your own home automation: connect remotely to your Home Assistant instance without opening any ports using Cloudflare Tunnels. Tobias Brenner's Cloudflared add-on for Home Assistant makes this a breeze."
social_image: /images/posts/socialmediacards/cloudflare_tunnels_card.png
---

I run home automation with [Home Assistant][1] on a Raspberry Pi, and I'd always been a little uneasy
about exposing my home network to the web for remote access. I'd set up SSL encryption and all that,
but still felt uncomfortable having an open port in my router and to manage update and security for
such an important and private service.

I recently learned about [Cloudflare Tunnel][2], part of Cloudflare's [Zero Trust][3] offerings.
Cloudflare Tunnel provides a secure way to connect your resources to Cloudflare without requiring a
publicly routable IP address. Instead of sending traffic to an external IP and opening ports in your
firewall, a lightweight daemon in your infrastructure (`cloudflared`) creates outbound-only
connections to Cloudflare's global network. This is essentially an agent-initiated reverse tunneling
service.

<img src="/images/posts/cloudflare-tunnel-diagram.png" alt="Cloudflare Tunnel diagram" style="max-width:100%">

The service can connect HTTP web servers, SSH servers, remote desktops, and various other protocols
safely to Cloudflare's edge. All you need is a domain handled by Cloudflare.

When I learned about this technology, it immediately caught my attention as a way better solution
for securely accessing my Home Assistant setup. I also immediately discovered that this idea is far
from novel and that there's [an extremely awesome Home Assistant add-on by Tobias
Brenner][4] that makes this setup a breeze.

With the add-on, the changes necessary to get this running were surprisingly minimal, since I
already used Cloudflare for my domains anyway. I spent more time cleaning up my old SSL Cert and
renewing setup than I did this solution.

Cloudflare-side, it's essentially just creating the tunnel in Cloudflare and generating a secret token to provide to the add-on:

<img src="/images/posts/cloudflare-tunnels-ui.png" alt="Cloudflare Tunnels UI" style="max-width:100%">

Back in Home Assistant, you just need to allow requests from the Cloudflared add-on, which runs in a Docker container:

```yaml
http:
  use_x_forwarded_for: true
  # The Cloudflared add-on runs locally, so HA has to trust the Docker network it runs on.
  trusted_proxies:
    - 172.30.33.0/24
```

With this configuration, I then closed all previously forwarded ports in my router. Now my home
automation is accessible only through Cloudflare's secure infrastructure while being completely
sealed from direct access from the outside. After putting the cherry on top - enabling 2FA for Home
Assistant - I feel much better about it all.

I've struggled a bit with finding a way to close off all *internal* access to Home Assistant
(forcing it all through Cloudflare), but I'll keep at it.

If you run a Home Assistant instance, I highly recommend this setup.

[1]: https://www.home-assistant.io/
[2]: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps
[3]: https://www.cloudflare.com/learning/security/glossary/what-is-zero-trust/
[4]: https://github.com/brenner-tobias/addon-cloudflared
