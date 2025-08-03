# Jumpbox on VPS

> Your portal manager to other places

It's a machine responsible for allowing you to connect to your homelab from wherever you're in the world. I recommend setting it on a VPS in your country. It could be anywhere but you'd have to deal with long latencies. Since it's a public machine, you better take precautions when configuring it, and these are the steps for that:

- Figure out your public IP (`dig @resolver4.opendns.com myip.opendns.com +short -4`), and create a firewall that negates everything but connections on 22 coming from your IP
  - To test it you can route your mobile network from your phone and try connecting to the machine from there
  - If your machine is open, you'll eventually get random traffic from scanning bots! I didn't believe it until something logged into my machine because of that
- Install Fedora Cloud and make sure to activate the firewall rule right after its creation
- Run the bootstrap playbook against the `blink` host
