```
tip: 16
title: Account Multi-signature
author: Marcus Zhao(@zhaohong ) <zhaohong229@gmail.com> 
discussions to: https://github.com/tronprotocol/TIPs/issues/16
status: Final
type: Standards Track
category: TRC
created: 2018-12-27
```


## Simple Summary

This doc describes the  standard interface of Account Multi-signature


## Abstract

Standard transactions on cryptocurrency networks can be called single-signature transactions because they require only one digital signature for a transaction to be done. Multi-signature is the requirement that signatures of the transactions must reach the weight customized before they can be executed. \
The scheme includes three kinds of permission, owner-permission, witness-permission, and active-permission, where owner-permission has the authority to execute all contracts, witness-permission is used for generating blocks, and active-permission is custom permission (a combination of contracts permission sets)
 
**Scenario 1**: 

Alice is running a company, she creates an account as her company fund account. Alice adds Bob(Accountant), Carol(CFO) and Alice(CEO) into the owner-permission of her account. Bob's signature weight is 2, Carol's signature weight is 2, Alice's signature weight is 5. Owner-permission's signature weight threshold is 3. Alice's signature weight is bigger than the threshold(5>3), so her only signature is sufficient to make transactions.  Bob's signature weight is smaller than the threshold(2<3), to make a transaction, Bob needs Carol's or Alice's signature if Carol approves, the total signature weight is 2+2>3, so the transaction can be executed.
 

**Scenario 2**: 

(Previous Scenario)\
Alice has many TRX assets. One day, misfortune has come, Alice is dead due to an accident.  She is the only one who holds the private key of her account, so her assets will stay in that account forever, nobody can get it.\
(Current Scenario)\
Alice has many TRX assets.  She creates an active-permission for her account, adds her husband and son's addresses into the active-permission, and give the active-permission authority to operate her account. So after Alice no longer exists, her family members can still operate her account.

**Scenario 3**:

Alice is running a company, she creates an account as her business account. Alice creates an active-permission and adds Bob(Accountant), Carol(CFO) and Alice(CEO) into the active-permission of the account. Alice gives the active-permission authority to operate her business account. One day, Bob resigns. To keep Alice's account safe, Alice can remove Bob's account from the active-permission, then Bob can not operate her account anymore.

**Scenario 4**:

(Previous Scenario)\
Alice has a witness account, if she wants to deploy a node but doesn't know how to deploy, she needs to provide the account's private key to the program administrator.\
(Current Scenario) \
Alice can assign witness-permission to the administrator. Since the administrator only has the producing-block permission, there is no TRX transfer permission, and even if the private key of the administrator on the server is compromised, TRX will not be lost.


## Motivation

1. Support account Access Control;
2. An account can be controlled by several private keys, in case of private key lost;

## Methods

#### AccountPermissionUpdate
```

  AccountPermissionUpdateContract {
    bytes owner_address = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    Permission owner = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;  //Empty is invalidate
    Permission witness = 3;//Can be empty
    repeated Permission actives = TMRHFP8NJnjkK8qv4ZLYs2FNh7AU4FKn1f;//Empty is invalidate
  }
  * @param owner_address: TU1W7NgVtGGqcGktBYDv8Qi4nUg8FbnuaH
  * @param owner :TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  * @param witness :TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  * @param actives :TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  * @return The transaction TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
 
 
  Permission {
    enum PermissionType {
      Owner = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
      Witness = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
      Active = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    }
    PermissionType type = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    int32 id = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;     //Owner id=TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s, Witness id=TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s, Active id start by TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
    string permission_name = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    int64 threshold = 1;
    int32 parent_id = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    bytes operations = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;   //1 bit TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s contract
    repeated Key keys = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
  }
  * @param type : Permission type, currently only supports three kind of permissions
  * @param id : Value is automatically set by the system
  * @param permission_name :TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  * @param threshold : Threshold, the corresponding operation is allowed only when the sum of the weights of the participating signatures exceeds the domain value.
  * @param parent_id : 1
  * @param operations : 1
  * @param keys : 1
  
  
  Key {
    bytes address = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    int64 weight = ;
  }
  * @param address : TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  * @param weight : TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  
```
#### GetTransactionSignWeight
 * @param transaction 
 * @return The transaction sign weight
 
```
TransactionSignWeight {TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
  message Result {TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s
    enum response_code {
      ENOUGH_PERMISSION = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
      NOT_ENOUGH_PERMISSION = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s; 
      SIGNATURE_FORMAT_ERROR = 2;
      COMPUTE_ADDRESS_ERROR = 3;
      PERMISSION_ERROR = 4; //The key is not in permission
      OTHER_ERROR = 20;
    }
    response_code code = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
    string message = 2;
  }

  Permission permission = TNeeVurmzvoGrZNxpCd9ceg3EJ9JdSAB3s;
  repeated bytes approved_list = 2;
  int64 current_weight = 3;
  Result result = 4;
  TransactionExtention transaction = 5;
}

```

#### AddSign
 * @param transaction 
 * @return The transaction

