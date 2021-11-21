---
title: "Simple Blockchain Implementation"
published: true
---
<p style="text-align:left; color:white">
<a href="/2021/11/19/Amfoss/index.html" >< Introspection</a>
</p>
<br/>

## [Task-12 (Make the Block)](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-12)
This task was very interesting and i would love to learn and do more projects on blockchain. Blockchain is something that is to be implemented on most of day to day life cases. Blockchain uses a decentralised system which make it impossible for someone to change the data. Since the ledger is distributed , change in one database doesnt effect others and data stay intact. Each Block contains its own as well as previous block's unique hash code which gives us a timeline from the initial transaction. Ive more to learn on mining and proof of works...etc. I chose java for this task because i found a lot of tutorial on that and i wanted to improve my java coding skills.
### Code:
```java
import java.util.Arrays;
import java.util.Scanner;

public class block {

    private int previousHash;
    private String[] transaction;

    private  int blockHash;
    private String[] transactions;

    public block(int previousHash, String[] transactions) {
        this.previousHash = previousHash;
        this.transactions = transactions;

        Object[] contents  = {Arrays.hashCode(transactions),previousHash};
        this.blockHash = Arrays.hashCode(contents);
    }

    public int getPreviousHash() {
        return previousHash;
    }

    public String[] getTransaction() {
        return transactions;
    }

    public int getBlockHash() {
        return blockHash;
    }
}

public class main {

    public static void print(int hash, int previoushash , String[] message ,int  n){
        System.out.println("block-"+n);
        System.out.println("Hash:");
        System.out.println(hash);
        System.out.println("Previous Hash:");
        System.out.println(previoushash);
        System.out.println("Message:");
        System.out.println(message[0]);
        System.out.println(" ");
    }

    ArrayList<block> blockchain = new ArrayList<>();
    public static <string> void main(String[] args) {
        System.out.println("Enter no of blocks:");
        Scanner input = new Scanner(System.in);
        int t = input.nextInt();
        String[] messages = new String[t];

        for (int i=0 ; i<t ; i++){
            System.out.println("Enter message in "+i+"th block:");
            Scanner block = new Scanner(System.in);
            messages[i]=block.nextLine();
        }

        while (true){
            String[] genesisTransactions = {messages[0]};
            block genesisblock = new block( 0, genesisTransactions);
            print(genesisblock.getBlockHash(), 0,genesisTransactions,0);
            int  pblck = genesisblock.getBlockHash();
            String[] ptrans = genesisTransactions;
            for (int i=1 ; i<t ; i++){
                String[] Transactions = {messages[i]};
                block nblock = new block( pblck, Transactions);
                print(nblock.getBlockHash(), pblck,Transactions,i);
                pblck = nblock.getBlockHash();
                ptrans = Transactions;
            }
            System.out.println("Enter no of block to change the message or -1 to exit: ");
            Scanner inp = new Scanner(System.in);
            int option = inp.nextInt();
            if(option == -1){
                System.exit(0);
            }
            else{
                if (option >= 0 && option < t){
                    System.out.println("Enter new message:");
                    Scanner message = new Scanner(System.in);
                    String mess = message.nextLine();
                    messages[option] = mess;
                }
            }
        }

    }
}
```

### Output :

```
Enter no of blocks:
4
Enter message in 0th block:
Message-1
Enter message in 1th block:
Message-2
Enter message in 2th block:
Message-3
Enter message in 3th block:
Message-4
block-0
Hash:
302694679
Previous Hash:
0
Message:
Message-1
 
block-1
Hash:
605389389
Previous Hash:
302694679
Message:
Message-2
 
block-2
Hash:
908084130
Previous Hash:
605389389
Message:
Message-3
 
block-3
Hash:
1210778902
Previous Hash:
908084130
Message:
Message-4
 
Enter no of block to change the message or -1 to exit: 
2
Enter new message:
Message-X
block-0
Hash:
302694679
Previous Hash:
0
Message:
Message-1
 
block-1
Hash:
605389389
Previous Hash:
302694679
Message:
Message-2
 
block-2
Hash:
908085277
Previous Hash:
605389389
Message:
Message-X
 
block-3
Hash:
1210780049
Previous Hash:
908085277
Message:
Message-4
 
Enter no of block to change the message or -1 to exit: 
-1
```


<br/>
<p style="text-align:left;">
<a href="/2021/09/30/htmljs/index.html" >< Previous</a>
<span style="float:right;"><a href="/2021/11/12/lemesee/index.html" >Next ></a>
</span>
</p>
