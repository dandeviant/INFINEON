
---

## 1.2. Preparing Catalyst 9K Platforms

### 1.2.1. Common configuration


> [!Note]- **Define HTTP source interface** 
> ```
> ip http client source-interface <NOC_mgmt_VLAN_ID>
> ```
> - Management VLAN is always Vlan99

> [!Note]- **Disable revocation-check for SLA trustpoint**
> ```
> crypto pki trustpoint SLA-TrustPoint
> 	revocation-check none
> ```

