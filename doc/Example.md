# Bitcoin and Blockchain part VI:The Complete Example of How Bitcoin Works

In this part, we extend our Trumpy-builds-wall example to get a better understanding of the bitcoin system. This example is from the perspective of payment, which is the most common use case of bitcoin. Although we will cover most of the concepts we covered in previous parts, some of them might not be included in this example. We highly recommend going through those parts again in detail.

## Pay with bitcoin

As known by all, Trumpy wants to build a wall, keeping others from entering his backyard. He picks a brick of 0.75 bitcoin on the website called Bob's and is going to pay with bitcoin. He scans the QRcode using his bitcoin wallet app to complete the payment. The bitcoin system story begins from here.

After scanning the QRCode, Trumpy's bitcoin wallet software starts constructing the transaction that assigns bitcoins to Bob, the owner of the Bob's website. To construct the transaction, the wallet needs to create the transaction input and transaction output. As for input, the wallet will look for unspent transaction output assigned to Trumpy from transactions recorded in the blockchain. The wallet will use the UTXO that has the closest value to the desired amount, which is 0.75 bitcoin in our case. The closest UTXO that Trumpy has is 1 bitcoin, and it will be used in its entirety. This means that change back to Trumpy must be generated. Therefore, the transaction has one input and two outputs. 

![Alt Text](/images/Transaction-Trumpy-Pay.png)

As you can see from the above image, there are two outputs. One output is assigned to Bob's address and the other one is assigned back to Trumpy's address. The addresses are contained in the locking scripts in those two outputs. The sum of outputs is smaller than the input. The difference is transaction fee collected by a miner. 

Before sending this transaction, there is one more step that Trumpy's wallet software needs to do -- signing the transaction. Signing the transaction is actually signing the transaction input. The transaction input is an unspent transaction output locked by Trumpy's address. In order to spend it, the wallet software creates an unlocking script containing Trumpy's digital signature and adds it to the input. Finally, the transaction construction is completed and it is propagated onto the bitcoin network, waiting to be confirmed. 
## Transaction validation 

After being propagated by Trumpy's wallet software, the transaction assigning bitcoin to Bob will be validated by bitcoin nodes. One of the most important things to check is that whether the input of this transaction is from an unspent transaction output assigned to Trumpy. From the previous section, we can see that Trumpy provides his unlocking script to unlock the UTXO. Inside the UTXO, there is a locking script. The unlocking script contains Trumpy's signature and public key. The locking script contains an address. Mathematically, the relationship between private key, public key and address is public key = f(private key), address = g(public key). To verify the transaction, a node will check whether Trumpy's public key can generate the address. If it can, the nodes will propagate this transaction further. Otherwise, nodes will ignore this transaction.

## Transaction confirmation

There are special kinds of nodes in the bitcoin network called the miners. Miners compete with each other to solve the proof-of-work algorithm. Whoever wins the competition has the right to add a block to the blockchain. The new block from a winner also announces the beginning of the next competition. 

Ivanky is one of those miners who works very hard to solve the algorithm. While she is trying very hard to find the solution, she receives a new block from the network. She finds out this is a valid block. After adding it to her copy of blockchain, she creates a new block, giving up the previous competition. She also removes transactions that are contained in the block she just received, because those transactions are already in the blockchain, which means that they are confirmed. In her new block header, she adds a previous block hash pointing to the previous block in the blockchain. She also summarizes the transactions she has collected so far into a merkle tree root and then adds it to the new block header. Besides, she also calculates the difficulty target of the proof-of-work algorithm based on the following equation:

`difficulty = old difficulty * (actual time of last 2016 blocks / 20160 minutes), difficulty target = 2^(256 - difficulty)`

Finally, she sets the nonce field in the block header to zero, being ready to compete with other miners.

During the competition, Ivanky's specialized mining software hashes the whole block repeatedly by changing only the nonce randomly. Her goal is to find a hash result smaller than the difficulty target. Meanwhile, Ivanky receives Trumpy's transaction. Her mining software adds that transaction to the new block and updates the merkle tree root. Luckily, this time Ivanky wins the competition. Her mining software immediately propagates the new block onto the network, securing the victory and ending the war. 

Every full node will validate the block containing Trumpy's transaction. If it is valid, it will be added to their copy of the blockchain and propagated further. It won't be long before several blocks were appended to that block since a new block is mined every ten minutes. Once that has happened, Trumpy's transaction is considered to be confirmed. From this moment on, Bob can now spend his bitcoin receive from Trumpy.

## Spending bitcoins

Bob notices the transaction from Trumpy is confirmed. He is very happy and wants to grab a beer. He, again, decides to pay with his bitcoin. The same payment story goes on again from the beginning of this part. His new transaction will start from the output of another transaction assigning bitcoins to him, forming a transaction chain.

Not only do the transactions form a chain, the blocks also form a chain, as known as the blockchain. Once a transaction is added to the blockchain, it is very difficult to modify it or remove it. Because one has to compete with lots of miners to win the right to edit the blockchain. With more and more blocks are being appended to the blockchain, transactions in it will soon become permanent truths and the bitcoin system will gain more and more trust, making it a positive circle system. 

Bitcoin is just one of the many applications based on the blockchain technology. A technology with as much potential as the blockchain surely will have other applications based on it. And, as many are saying, we are just at the beginning.

## Note

This post is a part of the project called ["Bitcoin and Blockchain"](https://github.com/JunbangHuang/blockchain). Its purpose is to help people understand the detail of the bitcoin system without diving into any textbooks.

The view points of this project are mainly based on my own bitcoin system understanding, Jian Zhang' wonderful blog series -- ["The Secret of Bitcoin and the Blockchain"](http://www.infoq.com/cn/articles/bitcoin-and-block-chain-part01) and a great book -- [Mastering Bitcoin](http://shop.oreilly.com/product/0636920032281.do) by Andreas Antonopoulos. 

This post is revised by [Xiayang Fan](https://www.linkedin.com/in/xiayang-fan-023465a8/).
