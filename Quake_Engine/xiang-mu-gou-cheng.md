# 项目构成

整个Quake引擎由以下几个主要部分组成：

```
// Project Tree
Quake
|
├─ QW
│  ├─ client
│  ├─ docs
│  ├─ dxsdk
│  │  └─ sdk
│  │     ├─ inc
│  │     └─ lib
│  ├─ gas2masm
│  ├─ progs
│  ├─ qwfwd
│  ├─ scitech
│  │  ├─ include
│  │  └─ lib
│  │     └─ win32
│  │        └─ vc
│  └─ server
├─ qw-qc
└─ WinQuake
   ├─ data
   ├─ docs
   ├─ dxsdk
   │  └─ SDK
   │     ├─ INC
   │     └─ LIB
   ├─ gas2masm
   ├─ kit
   └─ scitech
      ├─ INCLUDE
      └─ LIB
         └─ WIN32
            └─ VC

```

1. **Quake World**这是一个用于测试`Quake`引擎的子项目，其中包括客户端和服务端，以及Dx SDK等等必要组件。
2. **qw-qc:**Quake引擎的内置模型以及`Windows`版本的`Quake`引擎



