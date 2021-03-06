# Smart Contract Interface

## ETH Mainnet

| Contract | Description | Address ||
| :--- | :--- | :--- | :--- |
| Curve <> Curve Pool Pipe | Rebalance liquidity between Curve pools | [0x83c32BF929F80e404ff30Ede7333271460b13cd9](https://etherscan.io/address/0x83c32BF929F80e404ff30Ede7333271460b13cd9) |  
| Uniswap <> Curve Pool Pipe | Rebalance liquidity between Uniswap & Curve pools | [0x66895417881B1d77Ca71bd9e5Ba1E729f3Aa44D3](https://etherscan.io/address/0x66895417881B1d77Ca71bd9e5Ba1E729f3Aa44D3) | 
| Uniswap <> Uniswap Pool Pipe | Rebalance liquidity between pools on Uniswap | [0xaecCd58001D52B4b931FD6FD5bF87D4F911100B7](https://etherscan.io/address/0xaecCd58001D52B4b931FD6FD5bF87D4F911100B7) | 

## Usage example (Curve to Curve Pipe)
#### Swap 5 CRV tokens (sUSD) for y Curve tokens

```
import Web3 from "web3";

// A copy of the contract ABI is required
  import curveCurvePipeABI from "../curveCurvePipe.json";
  import ERC20ABI from "../ERC20.json"
  
  const web3 = new Web3(provider.wallet.provider);
  const address = provider.address;
  const curveCurvePipeAddress = '0x66895417881B1d77Ca71bd9e5Ba1E729f3Aa44D3';
  const curveCurvePipeContract = new web3.eth.Contract(curveCurvePipeABI, curveCurvePipeAddress);
  const crvTokenContract = new web3.eth.Contract(ERC20ABI, fromCurveTokenAddress);
  const valueToSend = (5 * 10 ** 18).toFixed(0); //Sending 5 CRV tokens
  const tx = await curveCurvePipeContract.methods.Curve2Curve(
    address,
    "a5407eae9ba41422680e2e00537571bcc53efbfd", //sUSD curve pool
    valueToSend,
    "bbc81d23ea2c3ec7e56d39296f0cbb648873a5d3" //y Curve pool
  );
  const allowance = await getAllowance(web3, fromCurveTokenAddress, address, curveCurvePipeAddress);
  if (allowance < value) {
    const approvalAmount = '100000000000000000000'; 
    let approveTx = await crvTokenContract.methods.approve(
      curveCurvePipeAddress,
      web3.utils.toWei(approvalAmount, 'ether')
    );
    approveTx
      .send({
        from: address,
        gasPrice,
      })
      .on('transactionHash', async (txHash) => {
        await sendTransaction(address, 0, tx, gasPrice, 2000000); // Rely on nonce for tx ordering to avoid waiting for approval to confirm
      });
  } else await sendTransaction(address, 0, tx, gasPrice); // Contract already has approval, gas estimates will not fail
};
```


