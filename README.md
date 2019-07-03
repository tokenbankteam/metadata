
##测试环境

jungle测试网 合约 metadatatptp
节点：https://jungle2.cryptolions.io

##转账更新信息：

```
transfer(name from, name to, asset quantity, string memo)
update(name account_name,string title,string avatar,string desc,name modifier,string url)
```

###第一次更新信息：

```
cleos push action eosio.token transfer '["huoyantest11","metadatatptp","0.1000 EOS","huoyantest12"]' -p huoyantest11
cleos push action metadatatptp update '["huoyantest12","火焰神","http://www.huoyan.jpg","这是火焰之家","huoyantest11","\"web\":\"123\""]' -p huoyantest11
```

###第二次更新信息：

```
cleos push action eosio.token transfer '["huoyantest13","metadatatptp","0.1500 EOS","huoyantest12"]' -p huoyantest13
cleos push action metadatatptp update '["huoyantest12","火焰神13","http://www.huoyan.jpg","这是火焰之家13","huoyantest13","\"web\":\"13\""]' -p huoyantest13
```

###账号本人更新信息：

```
cleos push action eosio.token transfer '["huoyantest12","metadatatptp","0.0750 EOS","huoyantest12"]' -p huoyantest12
cleos push action metadatatptp update '["huoyantest12","火焰神本人","http://www.huoyan.jpg","这是火焰之家本人","huoyantest12","\"web\":\"本人\""]' -p huoyantest12
```

###账号本人二次更新信息：

```
cleos push action metadatatptp update '["huoyantest12","火焰神本人2","http://www.huoyan.jpg","这是火焰之家本人2","huoyantest12","\"web\":\"本人2\""]' -p huoyantest12
```

##增加审查人

```
cleos push action metadatatptp addverifier '["chendatony44"]' -p metadatatptp
cleos push action metadatatptp addverifier '["metadatatptp"]' -p metadatatptp

```

##申请审查

```
applyverify(name account_name,string memo)
cleos push action metadatatptp applyverify '["huoyantest12","请确认身份"]' -p huoyantest12
```

##审查

```
verify(name account_name)
cleos push action metadatatptp verify '["huoyantest12","metadatatptp"]' -p metadatatptp
```

##查询审查人

```
cleos get table metadatatptp metadatatptp verifiers

```


##查询账号信息

```
 struct [[eosio::table]] accounts {
            name account_name;
            string title;
            string avatar;
            string desc;
            name modifier;
            uint64_t status; //0:初值 1:已支付; 2:已修改; 3:account_name本人已经修改
            uint64_t verified;
            string url;
            asset price;
            uint64_t primary_key() const { return account_name.value; }
            uint64_t get_price()const {return price.amount; }
            uint64_t get_modifier()const {return modifier.value; }
            EOSLIB_SERIALIZE(accounts, (account_name)(title)(avatar)(desc)(modifier)(status)(verified)(url)(price))
        };
        typedef eosio::multi_index<"accounts"_n, accounts,
                        indexed_by<"price"_n,const_mem_fun<accounts,uint64_t,&accounts::get_price>>, //价格 索引
                        indexed_by<"modifier"_n,const_mem_fun<accounts,uint64_t,&accounts::get_modifier>>> account_table;  //修改者索引

cleos get table metadatatptp metadatatptp accounts

```

##查询审查申请

```
struct [[eosio::table]] investigate {
            name account_name;
            string memo;
            uint64_t propose_time;
            uint64_t primary_key() const { return account_name.value; }
            uint64_t get_propose_time() const { return propose_time; }
            EOSLIB_SERIALIZE(investigate, (account_name)(memo)(propose_time))
        };
        typedef eosio::multi_index<"investigate"_n, investigate,
                        indexed_by<"time"_n,const_mem_fun<investigate,uint64_t,&investigate::get_propose_time>>> investigate_table; //发起申请时间索引

cleos get table metadatatptp metadatatptp investigate

```


## 用户账号信息表字段规范

accounts

``` javascript
{
 account_name: "chendatony44",
 avatar: "https://tp-statics.tokenpocket.pro/website-token/1562040672940-tp-lab.png",
 desc: "小明的账号1",
 modifier: "chendatony44",
 price: "0.1000 EOS",
 status: 3,
 title: "小明",
 url: '{"website":"https://www.baidu.com","telegram":"tokenPocket_en","twitter":"TokenPocket_en","wechat":"TP-robot"}',
 verified: 0
}
```

