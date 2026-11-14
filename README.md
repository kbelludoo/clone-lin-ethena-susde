# clone-lin-ethena-susde

Experimental **LIN** clone of Ethena `StakedUSDe` — 8-hour linear reward vest
(`getUnvestedAmount` / `totalAssets`) plus ERC4626 `convertToShares` /
`convertToAssets` / preview mint-redeem.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [ethena-labs/code4arena-contest](https://github.com/ethena-labs/code4arena-contest) `7ffedb8873c2286930804e1c4feee0410fd0f033`
  - `protocols/USDe/contracts/StakedUSDe.sol` sha256 `e06068be04cb7fa579a8fe1a8afbea629e1bbb84d426fdace58187282a0af895` blob `0a56a7d18d308329e90b490428b8ee633a54dd70`
  - License GPL-3.0. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_ethena_susde.lin`](src/lin_ethena_susde.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/ethena_susde_c11.c`](test/oracles/ethena_susde_c11.c)
- **Transpilable kernel:** [`test/oracles/ethena_susde_kernel.c`](test/oracles/ethena_susde_kernel.c)
- **Proofs (lin-open only):** `examples/ethena_susde/` and `make ethena-susde-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production StakedUSDe replacement.

Named goldens (uint64-scale, vest=800, t=2 of 8 hours, balance=10800, supply=10000):

| id | call | value |
|---|---|---|
| unvested | elapsed=2h | **600** |
| totalAssets | balance-unvested | **10200** |
| convertToShares | assets=1020 | **1000** |
| convertToAssets | shares=1000 | **1020** |
| previewMint | shares=1 | **2** |
| deposit MIN_SHARES | empty vault assets=500 | **ok=0** |
| transferInRewards | still vesting at t=2 | **ok=0** |

`now` / `last_ts` / `vest_amt` / `balance` / `supply` are arguments (Solidity uses `block.timestamp`, storage `vestingAmount`, `IERC20.balanceOf`, `totalSupply`). `7e9*3e9` fail-closes while naive i64 wraps. Distinct from Sky sUSDS chi/RAY.

## Reproduce proofs (in lin-open)

```
make ethena-susde-proof
```

## Not claimed

Uint256 / mainnet stUSDe / IERC20 host / `block.timestamp` host / OZ virtual-offset convert.
LIN replacing Ethena, Sky, or the EVM. TVL or dollar savings. zk soundness.
Sky sUSDS chi/RAY. Hidden vault storage.
