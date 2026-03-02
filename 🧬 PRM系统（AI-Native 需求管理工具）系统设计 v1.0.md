# 🧬 PRM系统（AI-Native 需求管理工具）系统设计 v1.0

# 一、系统定位

system:

  name: AI Product Requirement Manager

  abbreviation: AI-PRM

  purpose: 结构化产品需求并驱动AI coding生成web系统

  target_users:

​    \- product_manager

​    \- tech_lead

​    \- ai_agent

# 二、系统核心层级结构（与你图完全对齐）

hierarchy:

- project
  - module
    - pages
      - user_flow
      - data_entities
      - state_machine
      - api
      - rules
      - edge_cases

# 三、数据库模型设计

## 1️⃣ Project 表

entity: project

fields:

- id: uuid
- name: string
- description: text
- owner: string
- created_at: datetime
- updated_at: datetime

## 2️⃣ Module 表

entity: module

fields:

- id: uuid
- project_id: uuid
- name: string
- description: text

## 3️⃣ Page 表

entity: page

fields:

- id: uuid
- module_id: uuid
- name: string
- route: string
- type: string  # table / form / detail / dashboard

## 4️⃣ 子结构统一存储

以下结构统一使用 JSON Schema 存储（让AI直接可读）：

- user_flow
- data_entities
- state_machine
- api
- rules
- edge_cases

entity: page_schema

fields:

- id: uuid
- page_id: uuid
- user_flow: json
- data_entities: json
- state_machine: json
- api: json
- rules: json
- edge_cases: json

# 四、前端系统结构设计

## 页面结构（左侧树 + 右侧编辑器）

左侧结构树（参考slack交互，多层级）：

Project

  └── Module

​        └── Pages

右侧编辑区：

- User Flow
  - 示例
    -   flow:
      - enter: creator_list_page
      - action: filter_creators
      - action: select_creators
      - action: add_to_campaign
      - action: send_sample
- Data Entities
  - 示例
    -   entities:
      - name: creator
      -   fields:
        - id: int
        - name: string
        - followers: int
        - avg_views: int
        - category: string
- State Machine
  - 示例
    -   state_machine:
      - ​    entity: campaign
      - ​    states:
        - ​      \- invited
        - ​      \- sample_sent
        - ​      \- content_posted
        - ​      \- payment_done
- API
  - 示例
    -  api:
      -  -name: get_creators
      -  method: GET
      -  path: /api/creators
      -  response:
      -    list: creator[]
- Rules
  - 示例
    -  rules:
    - one_creator_multiple_campaign: true
    - sample_required_before_post: true
- Edge Cases
  - 示例
    -  edge_cases:
    -  scenario: creator没有地址
    -  expected: 不允许寄样，并提示填写地址