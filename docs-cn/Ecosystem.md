---
layout: default
title: "生态(Ecosystem)"
---

# 生态(Ecosystem)

- [输入和字段(Inputs and Fields)](#inputs-and-fields)
- [翻译(Translations)](#translations)
- [数据提供器(Data Providers)](#data-providers)
- [杂项(Miscellaneous)](#miscellaneous)

## 表单输入框

- [vascofg/react-admin-color-input](https://github.com/vascofg/react-admin-color-input): 使用 [React Color](http://casesandberg.github.io/react-color/) 拾色器的颜色输入框
- [LoicMahieu/aor-tinymce-input](https://github.com/LoicMahieu/aor-tinymce-input): 一个 TinyMCE 组件, 用于编辑所见即所得的 HTML
- [vascofg/react-admin-date-inputs](https://github.com/vascofg/react-admin-date-inputs): 一套基于 [material-ui-pickers](https://material-ui-pickers.firebaseapp.com/) 的日期选择组件

## 翻译

详情请看 [翻译](./Translation.md#available-locales) page.

## 验证提供器(Authentication Providers)

* **[AWS Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/setting-up-the-javascript-sdk.html)**: [thedistance/ra-cognito](https://github.com/thedistance/ra-cognito)

## 数据提供器(Data Providers)

* **[Django Rest Framework](https://www.django-rest-framework.org/)**: [synaptic-cl/ra-data-drf](https://github.com/synaptic-cl/ra-data-drf)
* **[Epilogue](https://github.com/dchester/epilogue)**: [dunghuynh/aor-epilogue-client](https://github.com/dunghuynh/aor-epilogue-client)
* **[Feathersjs](http://www.feathersjs.com/)**: [josx/ra-data-feathers](https://github.com/josx/ra-data-feathers)
* **[Firebase](https://firebase.google.com/docs/database)**: [aymendhaya/ra-data-firebase-client](https://github.com/aymendhaya/ra-data-firebase-client).
* **[Firestore](https://firebase.google.com/docs/firestore)**: [rafalzawadzki/ra-data-firestore-client](https://github.com/rafalzawadzki/ra-data-firestore-client).
* **[GraphCool](http://www.graph.cool/)**: [marmelab/ra-data-graphcool](https://github.com/marmelab/react-admin/tree/master/packages/ra-data-graphcool) (uses [Apollo](http://www.apollodata.com/))
* **[GraphQL](http://graphql.org/)**: [marmelab/ra-data-graphql](https://github.com/marmelab/react-admin/tree/master/packages/ra-data-graphql) (uses [Apollo](http://www.apollodata.com/))
* **[HAL](http://stateless.co/hal_specification.html)**: [b-social/ra-data-hal](https://github.com/b-social/ra-data-hal)
* **[Hasura](https://github.com/hasura/graphql-engine)**: [hasura/ra-data-hasura](https://github.com/hasura/graphql-engine/tree/master/community/tools/ra-data-hasura)
* **[Hydra](http://www.hydra-cg.com/) / [JSON-LD](https://json-ld.org/)**: [api-platform/admin/hydra](https://github.com/api-platform/admin/blob/master/src/hydra/hydraClient.js)
* **[JSON API](http://jsonapi.org/)**: [henvo/ra-jsonapi-client](https://github.com/henvo/ra-jsonapi-client)
* **[JSON HAL](https://tools.ietf.org/html/draft-kelly-json-hal-08)**: [ra-data-json-hal](https://www.npmjs.com/package/ra-data-json-hal)
* **[JSON server](https://github.com/typicode/json-server)**: [marmelab/ra-data-json-server](https://github.com/marmelab/ra-data-json-server).
* **[Loopback](http://loopback.io/)**: [kimkha/aor-loopback](https://github.com/kimkha/aor-loopback)
* **[Parse Server](https://github.com/ParsePlatform/parse-server)**: [leperone/aor-parseserver-client](https://github.com/leperone/aor-parseserver-client)
* **[Parse](https://parseplatform.org/)**: [almahdi/ra-data-parse](https://github.com/almahdi/ra-data-parse)
* **[PostgREST](http://postgrest.com/en/v0.4/)**: [tomberek/aor-postgrest-client](https://github.com/tomberek/aor-postgrest-client)
* **[Prisma](https://github.com/weakky/ra-data-prisma)**: [weakky/ra-data-prisma](https://github.com/weakky/ra-data-prisma)
* **[REST-HAPI](https://github.com/JKHeadley/rest-hapi)**: [ra-data-rest-hapi](https://github.com/mkg20001/ra-data-rest-hapi)
* **[Sails.js](https://sailsjs.com/)**: [mpampin/ra-data-json-sails](https://github.com/mpampin/ra-data-json-sails)
* **[Spring Boot](https://spring.io/projects/spring-boot)**: [vishpat/ra-data-springboot-rest](https://github.com/vishpat/ra-data-springboot-rest) 
* **[Strapi](https://strapi.io/)**: [nazirov91/ra-strapi-rest](https://github.com/nazirov91/ra-strapi-rest)
* **[Xmysql](https://github.com/o1lab/xmysql)**: [soaserele/aor-xmysql](https://github.com/soaserele/aor-xmysql)
* Local JSON: [marmelab/ra-data-fakerest](https://github.com/marmelab/ra-data-fakerest)
* Simple REST: [marmelab/ra-data-simple-rest](https://github.com/marmelab/ra-data-simple-rest).

## UI

- [**Bootstrap**](https://getbootstrap.com/): [bootstrap-styled/react-admin](https://bootstrap-styled.github.io/react-admin)

## 杂项

- [marmelab/ra-realtime](https://github.com/marmelab/react-admin/tree/master/packages/ra-realtime): 开启实时更新
- [marmelab/ra-tree-ui-materialui](https://github.com/marmelab/react-admin/blob/master/packages/ra-tree-ui-materialui/): 呈现树形数据的组件. 这个包是我们 [实验室](/Labs.md) 的一部分实验. 意味着它可能缺失一些特性, 有些边界条件可能覆盖不全. 如果使用就要自己承担风险了. 同时, 我们也期待您的反馈!
- [marmelab/ra-tree-core](https://github.com/marmelab/react-admin/blob/master/packages/ra-tree-core/): 核心组件, 提供逻辑处理, 不涉及UI. 这个包是我们 [实验室](/Labs.md) 的一部分实验. 意味着它可能缺失一些特性, 有些边界条件可能覆盖不全. 如果使用就要自己承担风险了. 同时, 我们也期待您的反馈!
- [ra-customizable-datagrid](https://github.com/fizix-io/ra-customizable-datagrid): 可用来动态隐藏/展示列数据的插件.
- [api-platform/admin](https://api-platform.com/docs/admin): 建立一套完整特性的管理端, 用React Admin作为API支持[Hydra Core Vocabulary](http://www.hydra-cg.com/), 包括但不限于用[API Platform framework](https://api-platform.com)生成的 APIs
- [zifnab87/ra-component-factory](https://github.com/zifnab87/ra-component-factory): 对 不可变的/可视化 的字段/菜单链接/行为按钮, 简单的字段/属性重排, 基于角色权限的页签标识, 的中央配置.
- [ctbucha/bs-react-admin](https://github.com/ctbucha/bs-react-admin): [BuckleScript](https://bucklescript.github.io/) 绑定React Admin.
