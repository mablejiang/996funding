# Designing a cryptocurrency donation model for the Anti-996 movement

## Why do we create such a model

996.icu represents a type of new social organization – self-organized, open-sourced, anonymous, and decentralized. It’s a kind of collaboration that formed for a common goal (common within a certain period of time). Such reckless online organizations indeed made quite a bit of progress and received huge impact via github, slack, and other platforms, they still have their own weak spot. 

** This weak spot is funding.** Anything gets complicated when it comes to money. Money could pollute the originally simple cause, but without money, many things could only stay on paper or PR without actions – no real problems could be solved. Just think about it, any worker that wants to sue a company that runs with 996-system per the Labor Law, he/she could hardly afford the cost for the attorney, not to mention the opportunity cost that this person could not land a job anywhere later. Therefore, certain amount of economic incentives is reasonable, especially to encourage the avant gardes. 

So very few real-world options are left to us:
1.	**Founding an NGO**, abide the rules for NGO in terms of receiving funding, auditing, and publishing financials.
2.	Founder’s **private account**, such as Paypal, Patron, or bank account.
3.	**Cryptocurrency Address**, of which the private key is controlled by the fundraiser.

For temporary movements like Anti-996 that requires anonymity and timeliness, the NGO option is too costly and not efficient enough. The set-up will not complete before the media’s attention and public’s willingness to donate fade away. 

For option 2, there are two types of risks. One is that the founder’s own credit risk. If the financial data is not transparent enough, the community could easily suffer from internal distrust or even fight. The other is that the fundraiser ** to some extent would have to expose his/her identity**. The government could easily go after you. The internet surfers could also do the same. 

Option 3 guarantees the anonymity of donators and fundraisers, with completely transparent financial data. Everyone could check how much money is raised and where it is spent. However, given that the private key needs to be held in one person, the custody part is still centralized. The decision on where the money go and how much it is spent, is all by a centralized party – there is always room for fraud and cheating in the system even after introducing a voting mechanism.

Is there a way to find a more reasonable donation model, based on the option 3, by revising the economy design? 

Our goals are:
-	Transparent financial data
-	Asset security
-	Usage of donation traceable and democratized
-	Support anonymous donating and receiving

Without a perfect solution, we aim to get infinitely close towards the goals above, and try to minimize (rather than diminish) the risk from centralization. 

Inspired by some schools of thoughts in cryptocurrency and blockchain, we designed a simple economy model that could be realized in reality. 

## The simplest design

Via Bancor Protocol, we can easily realize a donation system that allows exchange / swap at anytime. Regarding Bancor, please see https://about.bancor.network/protocol/. 

For now, let’s abstract the model:

***Issuance of tokens**: issue a ERC20 token on Ethereum – e.g. 996.token
***Generate an account**: generate a connector using Bancor Protocol
***Confirm the number of tokens**: decide on the important metrics for the Bancor algorithm, deposit a certain amount of ETH into the connector balance, and deposit a certain amount supply of 996.tokens. 

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.14.png)

Bancor Protocol can realize a lot more complicated mathematical design and application scenarios. Here we set the Connector Weight =100% =1 in order to ensure that the donation has no speculation involved. As a result, the amount of ETH being deposited into the connector balance essentially sets the price for the 996.token.

**Please note that this price is not meaningful, as CW=1, when there is no change in the connector, the price will maintain the same.** The situations that the connector changes will be discussed shortly – but the price will only go lower, not the other way.

For example, the number of ETH we deposit into the connector is 100, and there are 1M 996.tokens. When CW=1, the token price equals to price = 100/1000000 = 0.00001ETH

Why do we set CW=1? Only when CW=1, the token price will not change as the donation supply changes. 

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.24.png)

- **Receiving Donation**: Donators deposit their ETH into the connector balance, and exchange certain amount of 996.tokens by the proportion calculated above. After this step, here we have the biggest difference from a traditional donation model:

- **You can exchange your 996.tokens at anytime to get back a part of the donation.** Given the connector is a counterparty that allows free exchange at moment, the donator could exchange back his/her ETH with his/her tokens at **the original price or a lower price.** This is an additional option compared to the traditional donation model. 

- **Taking away the donation**:
If the manager takes out the donation in batches without a set schedule, **then each time after some money being taken out, the token price will be impacted / diluted**. Yet, since our scenario is donation rather than investment, price has minimal impact on donator. 

There are two ways to take out the donation: 
 
1.	Taking out ETH from the connector balance directly
2.	Issue more tokens into the connector balance and exchange the ETH out with the newly issued tokens
Therefore, either the ETH in the connector balance decrease, or the tokens increase and are used to exchange for ETH. Either way has the same impact:

1.	Management takes out part of the donation
2.	The price of 996.token being diluted

In other words, taking out 20% ETH funding means the same as token price being diluted to 80% of the original price.

Given this model, the token price compared to the original price will only be lower. The speed of price dropping will be determined by the speed of fund being taken out, rather than by anything else like the speed of donation, etc., As far as the connector balance still has some ETH inside, donators could retrieve part of their donation (with a diluted price)

This connector, also a public key on Ethereum, by now becomes a donation account. All the time when and source where donation is made and spent will be publicly recorded on the ledger. As far as the fund is not retrieved by the management, donators can top down more or withdraw part of the original ETH. For example, when one sees that the donation is accumulated at a high speed and believes the funding is sufficient for the project, one can takes back the fund that he/she donated early on.

_What are the advantages of this model?_

-	**Good for decentralized / small-check-size donation_**
-	**Can keep anonymity all the time**
-	**Donators could have stronger supervision over the management**: if the management takes away all the money in a short period of time and even disappear, then as soon as this happen the donators who join later will be able to see this and stop donating given the distrust. On the other hand, if the management utilize the funding properly and reasonably, donators can always top down further or withdraw anytime they want. 
-	**Participation of governance**: in our current model, there’s no actual function for the 996.token. The token is more like a “receipt”. Yet, if we introduce on-chain governance, 996.token can be used for voting. For example, the core members have 80% whereas the donators have 20%, then they could together vote on a big decision or determine what to spend the proceeds for – the decision could only be effective when the vote reaches 51%+.
-	**Easy to fork**: given that any open source projects are open for anyone to contribute and collaborate, the initial proposer does not necessarily need to be the core member of the community, nor does this person need to be the fundraiser or fund manager. A big advantage of cryptocurrency model is that it could be easily forked. Whenever the community members or donators have doubt about things like the use of proceeds, and the problem cannot be solved by discussion, they could choose to fork and start a new donation address. As long as someone has the ability to win the majority’s trust, there could be multiple centers in one community, and this even lowers the concentration risk or the probability of black swan event. 

## More specific discussion

The model we discussed above is in its simplest form. Almost no mathematical or technical challenge exists in that one. How about the more complex and specific functions?

*Can we introduce a reward mechanism?

Above, in order to protect the “purity” of a not-for-profit project, we intentionally avoided the possibility of speculation. However, as Bancor Protocol is a general model that could be used for any kind of fundraising, value discovery can definitely be introduced into the system. 

When CW<100%, the connector becomes a leverage. As the supply of donation increases, the token price will be higher and higher; as more people sell out their tokens, the price falls down again. 

For instance, let’s say there is a special program under the Anti-996 Movement that needs fundraising. The initiator is A, the community members are B, and the donators are C. At the very beginning, A contributes 1 ETH to exchange for 100 996.tokens. C1 contributes 1ETH and gets 80 996.tokens. A then organized 10 Bs to kick off this project – each B receives 5 tokens (i.e. all Bs together receive 50 tokens). C2 continues to make contribution of 3ETH and receives 120 996.tokens. At this moment, A can exchange the 50 tokens he/she has left for the initial ETH he/she contributes, and so can B. 

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.33.png)

The curve could also look like this:
![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.39.png)

The parabolic function on the left can both incentivize the early participants and donators, and avoid speculation from the late-comers.

Such more sophisticated  design can essentially turn the community into a DAO by not relying completely on external donation. The early contributors and donators all receive certain rewards. The downside is the complication itself – and may introduce more speculative behaviors into the system that contaminates the community. 

Given that Anti-996 Campaign is still far from being mature, **we do not recommend such incentive to be introduced such a mechanism that is way too complicated in terms of the economic design, rules, and governance.** Still, it’s always good to brainstorm. 

* How do we avoid large corporation to 

We just mentioned that the tokens could be used for voting as a tool of online community governance. What if large companies, (e.g. the Chinese tech giants that are involved in the Anti-996 Campaigns), control a large amount of tokens by buying anonymously and attempt to manipulate the voting result? 

We believe that this could also be solved by some more detailed design or by forking. Technically, tokens do not necessarily need to be all exchanged by ETH donation – they could also be distributed to the core contributors in a centralized way, as far as the procedure is transparent and fair and no rule could be modified by one single party easily.

In terms of forking, if the large companies, out of evil purposes, monopolize the donation and voting pool, the community can always fork a new voting address and bancor curve. In this system, all information is symmetric, and the fairness could be sustained by the supervision and governance of the whole community.

* Can we use Bitcoin or other currency?

Given that Bitcoin is not turing-complete, the Bancor Protocol as of now could only be realized on ETH or EOS. These two chains also have relatively mature infrastructure built around. If someone is not fully content with the Bancor Protocol, he/she could always fork a modified version, given the code is not complicated.

Still, if the community would like to donate with Bitcoin, or exchange the ETH received into some stablecoins, they can use some other routes. As the most widely-used anonymous donation way is Bitcoin, the community members can also connect to the model we design here by swapping twice.

* How about that the private key is only controlled by one person?

In order to take out the donation from the Connector Balance, one person with the private key will need to go through the procedures. Yet, the private key can only controlled by one person – so this will become inevitably centralized. 

Knowing that we are not to design a perfect model, we believe that the whole procedure is already more transparent, more supervised, and more easily to be forked. 

Still, mechanically we should think about this problem in more creative ways. For example, if the Anti-996 community does not want to grow into a long-term NGO, but rather only want donation at certain time for certain purpose, we can totally turn the whole model into a single-budget-project. 

Let’s say A is an employee that sues his/her company somewhere and would like to raise funds for his attorney fees; B is an engineer, and would like to store all the 996-related evidence onto various clouds, or build a server himself/herself; C is a programmer / product manager that would like to organize a team to turn the model we describe above into a product. 

Then each of them could initiate a fundraising application – by application we mean a standalone Bancor curve and a public key address. The private keys are each controlled by the initiaters. These initiaters can not only set the parameters for their own model, but also set a different upper limit of funding that suits their own need – to ensure the donation is small, decentralized, and secure. The community management is only to audit the authenticity of the budget and endorse for the project / call for donation. The donators can choose to support any of the initiatives that they think meaningful, and to participate the community governance as they wish. 

They the whole community becomes an DAO rather than an NGO that relies on external funding. 

## Can we be more imaginative?

Okay now everyone knows that we are not only designing a model for the Anti-996 Campaign but rather **a cryptocurrency-based donation model that works for any not-for-profit - not just Anti-996**.

Of course, funding is not the only challenge that they face, but this problem is prevalent enough that pretty much most NGOs are facing. On the other hand, as cryptocurrency developed up till today, one of the largest problems still is that there is no application scenarios. There are so many experiments and products done and created for investment, speculation, and trading, but none of those really proved itself that what concrete value was brought to the world, including bitcoin. Or, we’d rather say, more as inspiration rather than realization. 

Some friends were asking me – 996, NGO, and Blockchain – these circles sound quite unrelated and may not be able to understand each other. Nevertheless, isn’t this also the beauty of an open-source world? What if someone that happens to cross all the fields someday sees this, and a miracle happens?




