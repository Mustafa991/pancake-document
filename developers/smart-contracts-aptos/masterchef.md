# MasterChef

## Contract info

**Contract name**: pancake::masterchef\
**Contract address:**&#x20;

7968a225eba6c99f5f1070aeec1b405757dee939eabcfda43ba91588bf5fccf3::masterchef

**Admin multi sig address**: eef8788f3eaa34b936d0bc5897fc40fa3782b1e663bf04edf2cd22fcd18329ff

[Aptos Explorer](https://explorer.aptoslabs.com/account/0x7968a225eba6c99f5f1070aeec1b405757dee939eabcfda43ba91588bf5fccf3/modules)

## Resources

### MasterChef

Th metadata of the masterchef.

```
struct MasterChef has key {
    signer_cap: account::SignerCapability,
    admin: address,
    upkeep_admin: address,
    lp_to_pid: TableWithLength<string::String, u64>,
    lps: vector<string::String>,
    pool_info: vector<PoolInfo>,
    total_regular_alloc_point: u64,
    total_special_alloc_point: u64,
    cake_per_second: u64,
    cake_rate_to_regular: u64,
    cake_rate_to_special: u64,
    last_upkeep_timestamp: u64,
    end_timestamp: u64
}
```

<table><thead><tr><th width="232">Name</th><th width="284.3333333333333"></th><th></th></tr></thead><tbody><tr><td>signer_cap</td><td><code>account::SingerCapability</code></td><td>The signer capability of the resource account.</td></tr><tr><td>admin</td><td><code>address</code></td><td>The admin address of the module.</td></tr><tr><td>upkeep_admin</td><td><code>address</code></td><td>The account that execute upkeep call.</td></tr><tr><td>lp_to_pid</td><td><code>table_with_length::TableWithLength</code></td><td>LP token type info and corresponding pool id.</td></tr><tr><td>lps</td><td><code>vector</code></td><td>All added LP token type info array.</td></tr><tr><td>pool_info</td><td><code>vector</code></td><td>A list of pool _info struct.</td></tr><tr><td>total_regular_alloc_point</td><td><code>u64</code></td><td>Sum of all regular farm allocate points.</td></tr><tr><td>total_special_alloc_point</td><td><code>u64</code></td><td>Sum of all special farm allocate points.</td></tr><tr><td>cake_per_second</td><td><code>u64</code></td><td>Cake reward per second for all farm pools.</td></tr><tr><td>cake_rate_to_regular</td><td><code>u64</code></td><td>The percentage of cake rewards that regular farm can earn</td></tr><tr><td>cake_rate_to_special</td><td><code>u64</code></td><td>The percentage of cake rewards that special farm can earn.</td></tr><tr><td>last_upkeep_timestamp</td><td><code>u64</code></td><td>The timestamp of the last upkeep execution.</td></tr><tr><td>end_timestamp</td><td><code>u64</code></td><td>The last time that farms can get cake reward, each upkeep operation will extend this timestamp.</td></tr></tbody></table>

### PoolUserInfo

All pool informations.

```
struct PoolUserInfo has key {
    pid_to_user_info: TableWithLength<u64, UserInfo>,
    pids: vector<u64>,
}
```

| Name                | Type                                 | Description                                             |
| ------------------- | ------------------------------------ | ------------------------------------------------------- |
| pid\_to\_user\_info | `table_with_length::TableWithLength` | A table contains `userInfo` corresponds to the pool id. |
| pids                | `vector`                             | A list of pool id.                                      |

### UserInfo

The user information in each pool.

```
struct UserInfo has store {
    amount: u128,
    reward_debt: u128
}
```

| Name         | Type   | Description                                         |
| ------------ | ------ | --------------------------------------------------- |
| amount       | `u128` | The total amount of token staked in the pool.       |
| reward\_debt | `u128` | The total amount of reward distributed to the user. |

### PoolInfo

The information of each pool

```
struct PoolInfo has store {
    total_amount: u128,
    acc_cake_per_share: u128,
    last_reward_timestamp: u64,
    alloc_point: u64,
    is_regular: bool
}
```

| Name                    | Type   | Description                                                      |
| ----------------------- | ------ | ---------------------------------------------------------------- |
| total\_amount           | `u128` | The total amount of stake token.                                 |
| acc\_cake\_per\_share   | `u128` | The accumulated cake per  share in the pool.                     |
| last\_reward\_timestamp | `u64`  | The latest time when reward is distributed.                      |
| alloc\_point            | `u64`  | The pool allocated point.                                        |
| is\_regular             | `bool` | True means this pool is regular farm, otherwise is special farm. |

## Entry Functions

### Deposit

Deposit the stake token into the pool. It will also transfer reward token to the user if there's any.

```
public entry fun deposit<CoinType>(
    sender: &signer,
    amount: u64
)
```

| Name   | Type     | Description                                    |
| ------ | -------- | ---------------------------------------------- |
| sender | `signer` | The sender's signer when calling the function. |
| amount | `u64`    | The amount of token that will be deposited.    |

### Withdraw

Withdraw the stake token from the pool. It will also transfer reward token to the user if there's any.

```
public entry fun withdraw<CoinType>(
    sender: &signer,
    amount: u64
)
```

| Name   | Type     | Description                                    |
| ------ | -------- | ---------------------------------------------- |
| sender | `signer` | The sender's signer when calling the function. |
| amount | `u64`    | The amount of token that will be withdrawn.    |

### Emergency Withdraw

Withdraw the stake token from the pool regardless of any rewards.&#x20;

```
public entry fun emergency_withdraw<CoinType>(sender: &signer)
```

| Name   | Type     | Description                                    |
| ------ | -------- | ---------------------------------------------- |
| sender | `signer` | The sender's signer when calling the function. |

### Mass Update Pools

Update all the pool information.

```
public entry fun mass_update_pools()
```

### Update Pool

Update a specific pool information.

```
public entry fun update_pool(pid: u64)
```

| Name | Type  | Description         |
| ---- | ----- | ------------------- |
| pid  | `u64` | The id of the pool. |

## Public Function

### Pending Cake

The pending cake reward of the user.

```
public fun pending_cake(
    pid: u64,
    user: address
): u64 
```

#### Input Values

| Name | Type      | Description                   |
| ---- | --------- | ----------------------------- |
| pid  | `u64`     | The id of the pool.           |
| user | `address` | The address of the depositor. |

#### Return Values

| Type  | Description                      |
| ----- | -------------------------------- |
| `u64` | The total amount of cake reward. |

### Pool Length

The total number of pools.

```
public fun pool_length():u64
```

#### Return Values

| Type  | Description                |
| ----- | -------------------------- |
| `u64` | The total number of pools. |

## Audit

OtterSec's PancakeSwap Aptos MasterChef security audit:

{% file src="../../.gitbook/assets/PancakeSwap_aptos_masterchef_audit.pdf" %}
