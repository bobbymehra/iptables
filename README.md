# iptables

iptables is a command-line firewall utility that uses policy chains to allow or block traffic. When a connection tries to establish itself on your system, iptables looks for a rule in its list to match it to. If it doesn't find one, it resorts to the default action.
iptables almost always come pre-installed on any Linux distribution

iptables uses three different chains: input, forward, and output.

1. Input:
Contains strict rules to prevent some evil-doers on the internet from harming our computers. If you want to open/block a port, this is where you’d do it.
For example, if a user attempts to SSH into your PC/server, iptables will attempt to match the IP address and port to a rule in the input chain.

2. Forward:
This chain is responsible for packet forwarding. Which is what the name suggests. We may want to treat a computer as a router and this is where some rules might apply to do the job.
Think of a router - data is always being sent to it but rarely actually destined for the router itself; the data is just forwarded to its target. Unless you're doing some kind of routing, NATing, or something else on your system that requires forwarding, you won't even use this chain.

3. Output:
You can’t send a single packet without this chain allowing it. You have a lot of options whether you want to allow a port to communicate or not. It’s the best place to limit your outbound traffic if you’re not sure what port each application is communicating through.
if you try to ping example.com, iptables will check its output chain to see what the rules are regarding ping and example.com before making a decision to allow or deny the connection attempt.

~~~
# iptables -L -n -v          // to list current iptables configuration
~~~

- Connection-specific responses:
  With your default chain policies configured, you can start adding rules to iptables so it knows what to do when it encounters a connection from or to a particular IP address or port.
  Below are the three most commonly used responses
  - Accept: Allow the connection.
    ~~~
    # iptables --policy INPUT ACCEPT
    # iptables --policy OUTPUT ACCEPT
    # iptables --policy REJECT ACCEPT
    ~~~
  - Drop: Drop the connection, act like it never happened.
    ~~~
    # iptables --policy INPUT DROP
    # iptables --policy OUTPUT DROP
    # iptables --policy REJECT DROP
    ~~~
  - Reject: Don't allow the connection, but send back an error. In this way, the end user will get to know that the connection is blocked in iptables.
    ~~~
    # iptables --policy INPUT REJECT
    # iptables --policy OUTPUT REJECT
    # iptables --policy REJECT REJECT
    ~~~

    - Examples:
      - Dropping connection from single IP:
        ~~~
        # iptables -A INPUT -s 10.10.10.10 -j DROP
        ~~~
      - Dropping connection from a range of IP addresses:
        ~~~
        # iptables -A INPUT -s 10.10.10.0/24 -j DROP
        ~~~
      - Example on how to block shh port:
        ~~~
        # iptables -A INPUT -p tcp --dport ssh -s 10.10.10.10 -j DROP
        ~~~
        
