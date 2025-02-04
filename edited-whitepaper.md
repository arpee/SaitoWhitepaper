Some editor notes:

-----------------------------------------------------

There may be technical inaccuracies. My strategy in editing was to fully understand as I read and change content to better enable the understanding from a newbie's POV. I tried not to change content without feeling I understood it, but someone more experienced will have to be the final auditor.

Whole paper changed to third person, as most of it was in third person. That means replacing use of 'we, us, etc.' with more omniscient diction.

Use of quotes for invented terms is only used now before the term is defined. Once a term has been defined its future occurrences in the paper are un-quoted.

Structure remains mostly the same. Most edits are within single sentences and keep the meaning but elaborate.

*characters between stars indicate italics*

content in funny brackets []{} are notes not intended to be in the final paper, they are usually questions about if an edit is accurate or acceptable

footnotes were not looked at in this edit

-----------------------------------------------------

Saito fixes the collective action problems that impede scaling in proof-of-work and proof-of-stake blockchains by coupling a finite ledger to a consensus mechanism that incentivizes the collection and sharing of transaction requests and the fees associated with them. The resulting network pays not just for mining and staking but for all activities that contribute economic value to the network. In the process, Saito fully eliminates majoritarian as well as other attacks.

Saito pays for user-serving infrastructure nodes from an open consensus mechanism. This creates economic motivation for the network to scale with new users and can be used to build secure decentralized versions of many data heavy services, such as un-astroturfable data exchanges, authentication and monetization applications, distributed key registries, payment channels and much more.

In economic terms, Saito can be understood as a solution for inducing a free market to deliver a public good, much like the vision of Bitcoin. Unlike classical crypto-currencies, Saito's design corrects for collective action problems inherent in proof-of-work and proof-of-stake consensus; the competitive focus is shifted away from work/stake and to transaction fee collection, so that profit-seeking actors strive to bring as many transactions into the system as they can, enabling natural and robust scalability. Indeed, estimates indicate that the practical limit for a Saito blockchain today is in the order of 100TB of data per day, and advances in routing capacity will allow it to reach the petabyte level within a decade - all thanks to the fact that Saito is bottle-necked by hardware, not its own consensus mechanisms.

The next section describes briefly the economic problems that need to be solved to build a scalable blockchain. The following sections outline how Saito solves these problems and describes implementations of these methods.

1. THE PROBLEM

The problem with blockchain scaling is not at the network technology layer: at the time of writing, data centers around the world are implementing 400Gbps network switches while 100Gbps connections are becoming standard even in lower-tier facilities. Given the resources to pay for the necessary equipment, the only constraints come from current consensus mechanisms which prevent a blockchain from being as decentralized, open and efficient as the public Internet backbone.

What limits network growth is the challenge of the network paying for itself. In the past, non-economists have waved away this limitation, claiming that as long as *someone* is earning money from the network they will, or must already be, paying the costs or performing the tasks necessary to support it. But this is not the case - proof-of-work and proof-of-stake are afflicted by two market failures: a tragedy-of-the-commons problem that leads to blockchain bloat and eventual collapse, and a free-rider problem that leads to an under-provision of user-serving infrastructure with an over-provision of paid activities like staking and mining. Neither problem is crippling at small scales, but they grow incapacitating as bandwidth and storage costs rise with the growth of the network. 

Are there alternatives? Faced with the need to pay for uncompensated network infrastructure, computer scientists throw the problem to the market. As economists have known since the 1960s [reference? Olson, M. The Logic of Collective Action. 1965], asking the private sector to fund non-excludable infrastructure requires closure somewhere in the economic model. If profit seeking actors receive their earnings from only one aspect of a system, they will over-optimize that aspect while keeping other facets of the system on life support. Actors that make earnings from fee collection will naturally attempt to keep restricted access to fees which they are working on unless they are incentivized to do otherwise. In proof-of-work and proof-of-stake the only explicit incentives come from mining and staking - hoarding transactions is, therefore, the most competitive strategy, as it gives competing miners or stakers more potential reward at the same cost, therefore less reason to compete. This dynamic undermines the openness of the consensus layer.

The only viable solution is to eliminate these market failures on the incentive level. Before the solution can be understood, the issues must be seen clearly. The major issues are as follows:

The tragedy-of-the-commons issue is created by the existence of a permanent ledger, which encourages nodes to accept payment now for storage costs that can be offloaded to others in the future. This incentive leads to bloated blockchains, and more subtly to transaction mis-pricing, as users can pay fees that do not reflect the cost of their transaction to network across time. The fact that this is a fundamental problem is self-evident from the way Satoshi's solution is "not to care," [citation - Bitcoin Whitepaper Section 7?] an approach that stops being viable in networks that operate at serious economic scale.

Eliminating the tragedy-of-the-commons problem requires all nodes that submit transactions bear the cost of processing those transactions for as long as they remain on the blockchain. In practice, this requires a market mechanism for accurately determining the price of on-chain data storage. It also requires eliminating blockchain creep or deferring fee collection so that payments are meted-out over time as the node continues to do work for payment rather than getting it upfront with the opportunity to pass work onto others. One solution to this problem is described in section 2.

The free-rider problem is more insidious and arises in blockchains where payments are made for one specific type of work, like mining or staking, at the expense of other necessary activities. This mismatch incentivizes participants to maximize their resources on paid work and minimize them on anything else. In the blockchain space, this results in miners and stakers "free-riding" on those who do the unpaid work of collecting transactions, developing applications, or otherwise supporting the user-facing network. The problem gets worse as the network scales, and evolutionary pressures make the trap inescapable: any Bitcoin miner who spends a smaller percentage of her revenue on hashing than her peers will lose market share and eventually capitulate.

In economics, the typical solution to the free-rider problem is to eliminate the property of "non-excludability" associated with any good or service: restricting its benefits to those who pay the cost of provision. This is impossible in the blockchain space without destroying the openness of the network, as it would require all users to run nodes to use the network. Computer scientists often address the problem by adding protective middle-ware, such as wrapping consensus payments in closed voting rings which themselves remain susceptible to the same economic attacks. The whack-a-mole style approach can never solve these economic problems; markets are powerful enough to undermine these structures when it is profitable to do so.

Without a solution to this problem the compromise resides between a network which cannot scale because it can only pay for a limited amount of network operations, or a network which scales but loses the open, decentralized, trustless qualities that make blockchain a useful invention. Neither approach is useful for building a genuinely open blockchain at a massive scale.

The theoretical solution to the free-rider problem requires eliminating the possibility for free-riding by fixing the underlying incentive structure so that participants are paid for providing what the network needs as a whole. Because the blockchain requires a quantifiable cost-of-attack, it is necessary to eliminate "mining" and "staking" as they are and shift to a different form of work that compensates nodes in proportion to the *value* they provide to the network rather than the amount of hashing or staking they do.

The Saito network uses a new approach to measuring the value of work done and pays nodes in proportion to this value. It derives the measure "work" from the transaction fees that users pay. The work of routing transactions into the network is the value on which the Saito network bases rewards. Honest nodes can be induced to do this work by expectation of a share of the fees collected. The challenge is to ensure this mechanism maintains a cost-of-attack hight than the possible rewards, such that attackers cannot spend their own money to attack the network and harvest it back in a perpetual loop. The security mechanism described in Section 3 outlines a technical method of accomplishing this.

1. FIXING THE TRAGEDY OF THE COMMONS

Saito solves blockchain creep by allowing nodes in the network to delete the oldest blocks in the ledger when a new block is accepted. Epoch length is specified in the consensus code and determines how many blocks are kept by nodes at one time. Different epoch window lengths may be suited to different applications: in an extreme case, a blockchain designed to handle global traffic for distributed key-exchange applications may have an epoch as short as 24 hours.

Saito specifies that once a block falls out of the current epoch, its unspent transactions outputs (UTXOs) are no longer spendable. However, any UTXO from that set which contains enough tokens to pay a rebroadcasting fee must be rebroadcast in the next new block. Block producers do this by creating "automatic transaction rebroadcasting" (ATR) transactions that include the original transaction data, but have entirely fresh and newly-spendable UTXOs which need not be rebroadcast for an epoch. After two epochs block producers may delete all block data from that first epoch, though the header hash may be retained to prove the connection with the original genesis block. 

The ATR mechanism fixes the tragedy of the commons problem completely, making it impossible for the blockchain to grow so large that it collapses. The key is ensuring that the rebroadcasting fee paid for by ATR transactions is a positive multiple of the average fee paid by new transactions over the previous epoch - in other words, the cost to rebroadcast can not be cheaper than the cost to add data to the chain in the newest epoch. As the blockchain expands and there is less space for new transactions available, market competition pushes up the fees paid by new transactions. This in turn forces up the rebroadcast fees and increases the amount of data pruned from the blockchain. The market reaches equilibrium when old data is removed at the same rate that new data is added. Although epoch length is a hard variable, the amount of data on chain at any time is a function of what users are willing to pay for fees, giving storage price on the chain this is a market determined price rather than an incidental one.

Market discovery of the true cost of blockchain processing is a side-effect of this incentive structure, which works by removing the incentive block producers have to add unprofitable data to the chain. This incentive mechanism replaces hard-coded economic variables which cannot meaningfully adapt to changing conditions in the market or network and it prevents pernicious forms of free-riding present in other chains, like deletion of on-chain data or refusing to participate in historical block storage despite earning network rewards. All such forms of cheating disappear, as nodes which do not store the whole blockchain are incapable of producing new blocks, as they do not know which payments must be rebroadcast and thus will fall out of consensus with nodes which are correctly rebroadcasting.

While this avoids the problem of the blockchain growing too large for network nodes to store, and ensures space on the blockchain can be priced accurately as hardware advances and demand changes, fixing the tragedy-of-the-commons problem does not address the issue of compensating nodes in the peer-to-peer network which pay for the auxiliary functions necessary to keep the network operating. To solve this problem, a new consensus mechanism is required.

1. ELIMINATING FREE-RIDING
[Figures in this section should be titled (in the image): "Routing Work Required"

In Saito any node can create a block at any time provided is has enough "routing work" accumulated for its proposed block, i.e. in its mempool. The amount of routing work required to produce a block depends on how quickly the proposed block follows the most recent one: consensus rules dictate that the work required starts high and decreases with time until it reaches zero. Since block producers will issue blocks as soon as it becomes profitable, the pace of block production is a function of the overall amount of routing work fulfilled by the network.

Figure 1: The Burn Fee Curve

Saito derives routing work from the transaction fee embedded in every transaction. Using this measure of work to produce blocks makes attacking the network expensive, since making claims about time cost money. It can be seen from Figure 2 that it is impossible for attackers to produce blocks at a faster rate than the main chain unless they have access to a larger pool of transaction fees. To secure this mechanism, Saito has routing nodes cryptographically sign transactions as they propagate through the network. Consensus rules specify that the amount of routing work a transaction provides any node drops with the number of hops in its routing path, and that transactions provide no usable routing work to nodes that are not in their routing path. The work used to produce blocks now becomes the efficient collection and sharing of inbound network fees.

Figure 2: Good Actor Burn Fee Cost

As long as there is no payment for block production, this system offers comparable security to Bitcoin: cost-of-attack can always be quantified and attackers must spend their own money to attack the chain. This allows users to wait however many block confirmations are needed to meet their security requirements. As a bonus, the network can increase the amount of routing work needed for block production to keep blocktime constant as transaction volume grows, so that security scales with fee-volume. 

The major problem with this approach lies in the consequences of requiring the network to burn fees to produce routing work.

Figure 3: Deflation from Burn Fee Over Time

Avoiding a deflationary crash requires the network to conserve its token supply, but fees from routing work cannot be given directly to block producers, as that would allow attackers to use the income from one block to generate the routing work needed to produce the next. Dividing up the payment between different nodes is preferable, but as long as block-producers have any influence over who gets paid an attacker can sybil the network or conduct grinding attacks that target the token-issuing mechanism.

Solving this problem requires inverting the classic proof-of-work solution. In Bitcoin, consensus rules make it expensive to produce blocks and fees are then handed to the block producer. This is meant to ensure block production is expensive but in reality guarantees there are always conditions under which attacks are profitable. In Saito the solution is the opposite. The first order-problem becomes securing the payment mechanism i.e. ensuring that payments are proportional to routing work regardless of who produced the block. The cost-of-attack that a mechanism with this property creates is then leveraged into being the cost of block production.

The trick is to ensure that there is always a quantifiable cost of attacking the system. The mechanism used to achieve this is dubbed the "golden ticket" solution. This mechanism pays honest nodes for collecting fees regardless of who produces blocks. Altogether, the solution is returning the transaction fees to the network through a process that cannot be gamed by any of the players in the system without them spending more money on the attack than they stand to gain via rewards. Implementation details are described in the next section.

1. THE GOLDEN TICKET

Whenever a node produces a block, it may collect the difference between the amount of routing work included in its mempool and the amount of routing work required for block production. No other payments are provided to block producers.

Unlocking those payments requires the network to solve a computational puzzle, the "golden ticket". This puzzle requires knowledge of the block hash to solve and cannot be calculated in advance. Miners on the network listen for blocks as they are produced and begin hashing in search of a solution. Should they find a solution it is propagated into the network as a normal fee-paying transaction.

Only one solution may be included in any block, and the solution must be to the previous block for the current block to be considered valid. If these conditions are violated or a golden ticket is not solved, the funds that were not paid out in the previous block are simply not allocated. They fall backwards into the blockchain and, reaching their epoch's end, are recollected by the consensus layer and redistributed as part of a future block reward.

Should a golden ticket be solved in time, the unallocated fees are released to the network, split between the miner that found the solution and a random node in the peer-to-peer routing network. The lucky routing node is selected using a random value sourced from the miner solution with each routing node's chance of winning normalized to be proportional to the amount of routing work it contributed to the solved block.

As transactions are routed into the network, it can be seen that the total claim on transaction fees (sum of routing work accumulated among all nodes in the routing path) grows while the amount of remaining work available shrinks. Attackers who use honest transactions to produce blocks must use their own tokens to compete with the routing work of honest nodes - and while this may increase their chance of winning the reward, they would have to spend proportionally *more* than their increased odds are worth, making attacks based on receiving one's own transactions a losing strategy.

The division of rewards between miners and routers, *the paysplit*, is by default set to 0.5 (half to miners, half to routers) but can be made adjustable as described in the section below. The golden ticket system can be visualized as follows:

Figure 4: The Golden Ticket System

This system has several major advantages over proof-of-work and proof-of-stake mechanisms. The most important is that Saito explicitly distributes fees to the nodes that service users, both by collecting transactions and producing blocks, and does so in proportion to the amount of value actors provide to the overall network. Network nodes compete for access to lucrative inbound transaction flow, and will happily fund whatever development activities are needed to get users on the network routing transactions through their nodes. Note, the services provided by edge nodes to attract Saito users can include public-facing infrastructure needed by other blockchains.

This is a fundamental shift. Where other blockchains explicitly define which activities have value, Saito lets the users signal what services provide value through fee-pricing, while the network infers who deserves payment. Saito directly incentivizes the efficient delivery of value to users by paying for all of what users value rather than a subset of network activities. Saito provides the only way to guarantee that a self-sufficient network can remain open and economically independent at scale.

The Saito consensus mechanism is also "twice as secure" as its proof-of-work and proof-of-stake counterparts. Honest nodes route transactions to block producers and earn fees in exchange, but attackers are thrown into an impossible situation: they must not only spend the same amount of fees as the honest network to produce a competitive chain, but must also match 100 percent of the mining output to find enough golden to ticket solutions to recapture their funds. For attackers to successfully launch attacks that reuse fees gained previously from the attack, they must out-mine the rest of the network to scrape their profits - adding significant cost and ensuring more money is spent attacking the network than the potential rewards of attack.

The basic version of the Saito system achieves 100 percent fee-security, meaning zero profit potential for consistent attackers, thus eliminating the fifty-one percent attack completely. Section 5 describes a modification to this mechanism that pushes security above 100 percent and guarantees that attackers will lose money in all circumstances. Regardless of which implementation is used, the economic problems created by mechanisms that rely on external supply-curves vanish: mining serves as a pure cost function instead of a difficulty function and the blockchain remains secure even if the supply curve for hashpower or stake becomes perfectly flat.

1. ADVANCED SECURITY - POWSPLIT

It is possible to increase cost of attack beyond 100 percent of available returns. Let us call this method "powsplit". Note that the Saito implementation has a *paysplit* of 0.5 and adjusts mining difficulty so that one solution is found per block, on average. Since miners cannot control the variance at which solutions are found, unexpected fluctuations in network hash power may create periods where network difficulty is lower than required for optimal security.

A ”powsplit” approach eliminates this problem by adjusting mining difficulty so that one solution is found every N blocks on average. When a previous block ,*n-1*, did not contain a valid solution, the random value used to pick the winning routing node is reused (hashed again) to select a new winner from the next preceding unsolved block, *n-2*. This ensures that despite golden ticket solutions occurring less frequently than blocks, routers still receive their split of rewards for each block. The remainder that would have been returned to miners should a golden ticket solution have been found, gets distributed to a 'staker'. The wining staker is selected from a staking table in a similar method to the selection of the winning router when a golden ticket is found. Staking is described in more detail below.

Stakers earn a split of network revenue from previous unsolved, unallocated blocks when a new block's golden ticket is finally solved. Stakers participate in the network by having a specially formatted UTXO placed into a "staking table". These UTXOs are first sent to a list of 'pending stakers' upon their inclusion into a block; the complete list of pending stakers is added to the staking table when the current staking table is fully paid out. To avoid throttling attacks on the staking mechanism it is wise to disallow stakers from spending or withdrawing staked UTXOs until they have received payment.

The percentage of network revenue allocated to staking nodes should be proportional to the amount of fees paid by the staking mechanism during the previous round. 
{"Should" without clear explanation}
{Richard's explanation if desired:
The reason this is true is that difficulty should be adjusting to create on average one golden ticket solution every other block. So in equilibrium the system will be paying out a constant number of times.

The statement is prescriptive, in that the staking rewards are a multiple of that paid out in the last round - this is to prevent entry exit attacks where the rewards can be inflated at some point in the future, and knowledge of that can be exploited to get an outsized reward.

This is deep weeds Saito, but the attack here would involve minder/staker collusion (or one actor being both secretly or otherwise) and deliberately flooding the staking table, and dropping all their hash (after dominating has - perhaps at a mild loss - and driving all other miners out of the system).

Done correctly - if the staking reward was proportinal to the ammount staked this round the attacker could manipulate that to get a positive reward (more than they spend orchestrating this attack)...
}
Limits may be put on the size of the staking pool to induce more competition between stakers or prevent users from spamming the staking mechanism in hopes of dissuading honest stakers from participating. In normal situations the finite blockchain and ATR mechanism will ensure that stakers who launch spam attacks will have a quantifiable cost in the form of rebroadcast fees.

[Foot](To ensure the system works, block producers who rebroadcast UTXOs must indicate in their ATR transactions whether the staking transaction outputs are in the current staking table or pending. A hash representation of the state of the staking table may be included in every block in the form of a commitment to allow nodes to verify the accuracy of the staking table, but the ATR rebroadcast mechanism will theoretically allow all nodes to reconstruct the state of the staking pool within the latest epoch.)

Mining difficulty can be adjusted upwards if two blocks each containing a golden ticket solution are found in a row or downwards if two blocks are found consecutively without a solution. A similar punitive mechanism can limit the staking payout if two consecutive blocks without golden tickets are found. Those interested in the underlying mathematics are encouraged to consult the team's papers on the topic {footnote with links}. The cost of attacking a Saito network using this mechanism rises significantly above 100 percent of the potential rewards.

Adding 'powsplit' to Saito Consensus guarantees that the cost of attack always exceeds potential rewards.

6. ADVANCED SECURITY PAYSPLIT

There are several modifications to the paysplit mechanism that can be used to increase attack costs. While the version of Saito being launched for production does not include this mechanism, it is possible to add a dynamic voting system to Saito that can allow paysplit to float dynamically. This section describes a theoretical improvement that allows for a floating paysplit that will work under certain assumptions about the rationality of the network.

An implementation of this system modifies blocks so that they include a vote to increase, decrease or hold constant the paysplit of the network. Golden ticket solutions may be then modified so that they contain a similar vote on the difficulty of the golden-ticket production function. The consensus variables of the network are updated when and only when golden tickets are solved and included in the blockchain.

Adjusting paysplit like this can change the distribution of fees between routing nodes and miners in real-time. This allows the network to reach an optimal equilibrium rather than an arbitrary one. To prevent this equilibrium from reflecting only the preferences of the routing and mining nodes, users on the network can tag their transactions with a paysplit vote as well: should a user-originated transaction contain such a vote, it may only be included in a block that votes in the same direction. Users who take sides in the ongoing struggle between routers and miners thus sacrifice the reliability and speed of transaction confirmation, but gain marginal influence over how the network allocates fees. Users making votes are also withholding their fees from nodes voting differently to themselves.

Under conditions where network participants exhibit bounded rationality, this mechanism pushes paysplit to the point where the security provided is optimal for all participants given the cost of additional fee collection. De Toqueville compacts secure the equilibrium: any two players in the tripartite network structure (routers, miners, users) may team-up to shift the paysplit back to the desired ideal. More research into this mechanism is planned in the future, but a useful thought experiment is exploring how the security of this three-player system degrades to only bitcoin-class security as the paysplit approaches extreme values.

7. ADDITIONAL NOTES ON NETWORK SECURITY

Saito’s design solves several long-standing problems of note. Hoarding attacks are minimized because nodes that participate in transaction routing maximize revenue by finding the most efficient routing path into the network. Competition encourages the sharing of fees rather than the hoarding of fees. The availability of routing information in blocks also allows participants to check that their peers are faithfully propagating their transactions instead of hoarding them.

Because adding hops to any routing path necessarily reduces the profitability of routing for every node in the path, sybil attacks are also eliminated. Blocks provide the information needed for participants to identify and eliminate sybils in their peer-to-peer networks. And evolutionary pressures ensures that they will purge them: weaker nodes which permit themselves to be sybilled will find themselves driven out of the network by competitive pressures over time.

The routing network also serves a unique defensive mechanism. Routing nodes in Saito can increase the cost of attack in real-time by refusing to route transactions to attackers, forcing an increased reliance by the attacker on their own wallet to fund block production. This mechanism also defends Saito against subtle attacks like monetizing transaction flows and closed-access routing.

As a final observation, note that the ”scalability trilemma” often championed as a fundamental law of blockchain does not exist in the Saito design. There are many obvious configurations of the network in which redirecting fees from miners to routing nodes can simultaneously increase the throughput, decentralization and security of the network simultaneously.

8. SUMMARY

The fundamental problems affecting blockchain scaling are economic. Saito fixes these issues, allowing us to build a massively-scalable blockchain which achieves scalability by ensuring that payments flow to the nodes that spend money on network infrastructure.

Those who pour over the technical details of the Saito network will find embedded in it at least seven major innovations in blockchain technology: automatic transaction rebroadcasting, the burn fee, the golden ticket system, paysplit and powsplit, N-block golden tickets, a secure multiparty voting mechanism, and the chain of cryptographic signatures that permits the blockchain to identify and reward productive nodes in the routing network.

Patent protection has been secured on these techniques and contact from other blockchain projects looking to incorporate one or several of these methods in their own networks is welcomed. Readers are also encouraged to visit *https://saito.io* which includes an interface for the working network, a roadmap outlining future development plans, and tutorials that can help anyone get started building Saito applications today.
