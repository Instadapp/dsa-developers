
# Using Tenderly to simulate a transaction

Tenderly is a Smart contract monitoring and alerting tool. With tenderly you can monitor smart contracts on multiple Ethereum networks. Recently, tenderly has come up with a great feature of simulating a transaction on Ethereum network which makes debugging a lot more easier than before.

So, today you will learn how you can simulate a transaction on tenderly.

**Step 1:** 

Create an account on [tenderly](https://dashboard.tenderly.co/register?utm_source=homepage)

**Step 2:** 

Select `Simulator` from the side bar 

![alt text]()

**Step 3:**

Create a new simulation using `New Simulation` button present on the right side of the screen.

Now, We will take a scenario from DSA which will throw an error of `gas required exceeds allowance` which is quite common. 

We will try to cast the oasis’ sell spell and maker’s borrow spell with amount  0.2 DAI and slippage 1%. 
![alt text]()

Now, If we will cast the above spells, then it will thrown an error of `gas required exceeds allowance`. But it doesn’t give any clue why it is throwing this error.

So, now we can use the DSA’s estimateCastGas() function which provide us the amount of gasLimit we need to provide to the spells and in its Catch section we will print the error which will give us the data variable  in case any error occurs.
![alt text]()

Now, after executing the dsa.estimateCastGas() function, it will return us error along with the data variable which we will use at tenderly.
![alt text]()

**Step: 4** 

In the new Simulation on tenderly, select option of `Use Custom Contract` and provide the `to` address from data variable to the `Address` section. 

Since, DSA is best functional on Mainnet we will select Mainnet in the Network section 

Now, take the value of `data` key from the data object in terminal and provide it to the `Raw Input Section` on tenderly.

In the Transaction Parameters change the `From` variable to your address which has DSA setup already. 

![alt text]()

Now, When we simulate the transaction it will tell us with the whole call stack and if the transaction will go through or not.

![alt text]()

In our case, it is showing that the transaction has failed along with an error `execution reverted`. 

But this time the error is quite understandable as it is providing us the condition which caused the error which in our case is `require(managerContract.count(address(this)) > 0, "no-vault-opened");`. Now, we can understand the error and work on solving it.

![alt text]()

You can also take a look at the whole Stack Trace which your transaction went through.

![alt text]()

If you want to know about the contracts that were involved in the transaction you can navigate to the `Contracts` section and take a look.


![alt text]()

There is also a feature of `Gas Profiler` which provides you with a gas usage breakdown by the function call.


Using these steps you can simulate a transaction on tenderly and debug your transaction.
