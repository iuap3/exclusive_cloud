﻿iuap-oid组件提供对实体唯一标识生成的统一封装，包含普通的UUID、基于Redis的自增ID、和snowflake、uapoid算法等。组件提供统一接口对OID进行生成，通过配置的方式实现对几种策略的切换。同时支持对自定义ID生成方案的扩展。

采用Redis自增方式，支持为不同模块设置不同的起始值。