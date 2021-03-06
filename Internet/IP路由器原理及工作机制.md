一个路由器其实就是一个超小型的电脑，而且操作系统大多为Linux，但是在做这些操作的时候你可能并不知道IP路由是如何工作的。本文将助你理解IP路由的原理，以及它是如何工作的。
　　一、IP路由涉及到IP报文的转发。如果主机与目的主机直接相连，那么主机可以直接发送IP报文到目的主机，这个过程比较简单。例如，通过点对点的链接或通过共享。如果主机与目的主机没有直接相连，那么主机会将IP报文发送给默认的路由器，然后由路由器来决定往哪发送IP报文。
　　二、普通的主机与路由器之间的根本区别在于，主机不会将一个报文从一个接口转发到另一个接口，而路由器可以转发报文，大多数的多用户系统都可以被配置，从而被当作路由器来用。
　　因此，一个普通路由算法可以被用在路由器上，同样也可以用在一台普通主机上。当一台主机可以用作路由器时，我们通常说这台主机嵌入了路由器的功能。这种具备嵌入路由器功能的主机平常不会转发报文，除非我们对它进行了配置，使它开启这种功能。
　　三、IP层维护着一张路由表，当收到数据报文时，它用此表来决策接下来应该做什么操作。当从网络侧接收到数据报文时，IP层首先会检查报文的IP地址是否是主机自身的地址相同，如果数据报文中的IP地址是主机自身的地址，那么报文将被发送到传输层相应的协议中去。
　　如果报文中的IP地址不是主机自身的地址，并且主机配置了具备路由的功能，那么报文将被转发;否则，报文就被丢弃。
　　四、路由表中的数据一般是以条目形式存在，路由表条目包含以下主要的条目项：
　　1、目的IP地址：此字段表示目标的IP地址。这个IP地址可以是某一台主机的地址，也可以是一个网络地址。如果这个条目包含的是一个主机地址，那么它的主机ID标记为非零;如果这个条目包含的是一个网络地址，那么它的主机ID被标记为零。
　　2、下一个路由器的IP地址：为什么我们使用“下一个”的说法，是因为下一个路由器并不总是最后的目的路由器，但它很可能是一个中间路由器。条目给出下一个路由器的地址是用来转发从相应接口收到的IP数据报文。
　　3、标志：这个字段提供了另一组重要，如目的IP地址(之前提到的)是一个主机地址还是一个网络地址。此外，从标志中可以得知下一个路由器(之前提到的)真的是一个路由器还是一个直接相连的接口。
　　4、网络接口规范：一些数据报文的网络接口规范，这个规范跟随报文一起传播。
　　五、如果我们现在想简单而形象地描述路由过程，我们将会看到：一旦主机(被配置成具备路由功能)的IP层接收到从网络侧来的数据报文，它将核实数据包中的目的IP地址，如果此IP不是主机的IP地址，那么包将通过路由表转发。
　　六、如果任何条目的第一个字段完全匹配目的IP地址或部分匹配目的IP地址，那么它将指示下一个路由器的IP地址。这是一个重要的信息，因为这些信息直接告诉主机数据包应该转发到哪一个“下一个路由器”去。而条目中所有其它的字段将提供更多辅助的信息来为路由转发做决定。
　　1、首先，路由表会去搜索一个“目的IP地址”字段与数据报文中目的IP地址完全相同的条目。这就意味着IP地址的主机ID与网络ID完全的匹配。如果找到，则数据包被发送到相应接口或中间路由器。
　　2、如果没有找到一个完全的匹配IP，那么就接着搜索相匹配的网络ID。如果找到，那么该数据报文会被转发到指定的路由器。所以我们看到，这个网络上的所有主机都通过这个路由表中的单个(这个)条目来管理。
　　3、如果上述步骤失败，即没有默认路由器，那么该数据报文最终无法被转发。任何无法投递的数据报文都将产生一个ICMP主机不可达或ICMP网络不可达的错误，并将此错误返回给生成此数据报文的应用程序。
　　在路由表中包含与网络相关的路由条目是一个很大的优点。其优点在于，拥有一个与完整网络相关的条目，能够避免包含此网络中所有单独的主机条目，这使得路由表的大小降到一个可收受的数量级，这样就非常好。
