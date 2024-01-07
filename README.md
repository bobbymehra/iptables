# iptables

iptables is a command-line firewall utility that uses policy chains to allow or block traffic. When a connection tries to establish itself on your system, iptables looks for a rule in its list to match it to. If it doesn't find one, it resorts to the default action.
iptables almost always come pre-installed on any Linux distribution

iptables uses three different chains: input, forward, and output.

- Input:
Contains strict rules to prevent some evil-doers on the internet from harming our computers. If you want to open/block a port, this is where you’d do it.
For example, if a user attempts to SSH into your PC/server, iptables will attempt to match the IP address and port to a rule in the input chain.

- Forward:
This chain is responsible for packet forwarding. Which is what the name suggests. We may want to treat a computer as a router and this is where some rules might apply to do the job.
Think of a router - data is always being sent to it but rarely actually destined for the router itself; the data is just forwarded to its target. Unless you're doing some kind of routing, NATing, or something else on your system that requires forwarding, you won't even use this chain.

- Output:
You can’t send a single packet without this chain allowing it. You have a lot of options whether you want to allow a port to communicate or not. It’s the best place to limit your outbound traffic if you’re not sure what port each application is communicating through.
if you try to ping example.com, iptables will check its output chain to see what the rules are regarding ping and example.com before making a decision to allow or deny the connection attempt.

~~~
# iptables -L -n -v          // to list current iptables configuration
~~~
