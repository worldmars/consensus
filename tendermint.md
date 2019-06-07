# 一、NewHeight

​	是否等待交易(意思是是创建空块的时间间隔)

​	是:设置一个定时时间 

​	否，进入NewRound

## 二、NewRound(proposal)

​    设置一个超时时间 TimeoutPropose + TimeoutProposeDelta*int64(round)

​	1、出块节点创建proposal

​		生成proposal(height, round, cs.ValidRound, blockhash, sign)

​		sign是height, round, cs.ValidRound, blockhash的签名

​	2、非出块节点接受proposal

    	条件:1、大于2/3票进入投票阶段
         	2、刚才设置的超时时间到了，进入投票阶段

## 三、Propose(进行投票)

​    投票内容: 投票类型(prevote\precommit\proposal)、block hash、height、round、address、sign(对前面的内容签名)

​	1、如果有锁定的block，投票给锁定的block

​	2、如果proposal为nil，投空票 即：blockhash 为nil

​	3、对proposal验证，失败投空票

​	4、投票给proposal

## 四、



## 超时时间设置

```go
TimeoutPropose        time.Duration `mapstructure:"timeout_propose"`
TimeoutProposeDelta   time.Duration `mapstructure:"timeout_propose_delta"`
TimeoutPrevote        time.Duration `mapstructure:"timeout_prevote"`
TimeoutPrevoteDelta   time.Duration `mapstructure:"timeout_prevote_delta"`
TimeoutPrecommit      time.Duration `mapstructure:"timeout_precommit"`
TimeoutPrecommitDelta time.Duration `mapstructure:"timeout_precommit_delta"`
TimeoutCommit         time.Duration `mapstructure:"timeout_commit"`

TimeoutPropose:              3000 * time.Millisecond,
TimeoutProposeDelta:         500 * time.Millisecond,
TimeoutPrevote:              1000 * time.Millisecond,
TimeoutPrevoteDelta:         500 * time.Millisecond,
TimeoutPrecommit:            1000 * time.Millisecond,
TimeoutPrecommitDelta:       500 * time.Millisecond,
TimeoutCommit:               1000 * time.Millisecond,
SkipTimeoutCommit:           false,
CreateEmptyBlocks:           true,
CreateEmptyBlocksInterval:   0 * time.Second,
```

