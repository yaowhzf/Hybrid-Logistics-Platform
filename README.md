# 混合智能驱动的物流调度仿真平台 (Hybrid-Intelligence-Driven Logistics Scheduling Simulation Platform)

本项目是一个“工业级原型”，旨在解决传统物流调度在“高动态”环境下的核心痛点。

我们不造“玩具”，我们只解决“真实世界”的问题。

## 1. 核心工业痛点 (The Core Industrial Pain Point)

在现代仓储和智能制造中，Google OR-Tools等传统运筹学（OR）求解器在离线全局路径规划上表现出色。

然而，当面临实时动态事件时（如：突然出现的障碍物、产线紧急插单、车辆故障），这些“纯OR”方案的“死穴”就暴露了：

    全局重规划 (Full Re-planning): 必须“停下全世界”，重新计算所有路径，导致系统**“分钟级”的卡顿和停摆**。

    缺乏适应性: 无法“毫秒级”地应对局部突发，效率低下。

一句话：传统方案在“不确定性”面前，既不“智能”，也不“高效”。

## 2. 我们的解决方案：“OR+RL”混合智能大脑

本项目抛弃了“纯OR”或“纯RL”的“玩具”方案，采用工业界前沿的**“混合智能” (Hybrid Intelligence)** 架构：

    离线层 (OR司令部): 使用 Google OR-Tools (VRP求解器)，计算“静态最优”的全局任务分配。它负责“战略”。

    实时层 (RL突击队): 使用 强化学习 (PPO/DQN) 训练的Agent，在ROS环境中，根据“实时”感知（CV、雷达），对“OR司令部”的指令进行**“毫秒级”**的动态修正。它负责“战术”。

简而言之：OR-Tools保证“全局最优”的下限，RL保证“动态适应”的上限。

## 3. 系统架构与技术栈 (Architecture & Tech Stack)

本平台基于 ROS (Robot Operating System) 作为“神经系统”，在 Gazebo 仿真环境中进行开发与验证。所有“智能”模块均被封装为“微服务”节点 (Node)。

    仿真/通信: ROS Noetic, Gazebo

    核心决策 (大脑): Python, Google OR-Tools, PyTorch/TensorFlow (RL)

    感知/交互 (眼睛/嘴巴): OpenCV (YOLOv8), FastAPI, LangChain (RAG)

    工程化 (部署): Docker, Docker Compose

模块划分 (Key Modules):

    [Gazebo仿真环境]: 模拟多AGV的仓库地图、货架、动态障碍物。

    [CV感知Node]: (订阅 /camera/image_raw) 运行YOLOv8，识别障碍物，发布 /obstacles。

    [定位Node]: (订阅 /odom) 模拟SLAM/AMCL，提供小车实时位姿。

    [OR司令部Node]: (离线API) 接收批量订单，调用OR-Tools，计算全局路径，发布 /global_plan。

    [RL突击队Node]: (核心) 订阅 /odom, /obstacles, /global_plan，实时输出“决策” /cmd_vel，实现动态避障和任务修正。

    [RAG交互API]: (可选亮点) 允许用户用“自然语言”下达“紧急插单”指令。

## 4. (预期) 核心价值：A/B 对比 ( (Expected) Core Value)

本项目将通过A/B测试量化“混合智能”的价值。


## 5. 项目状态 (Project Status) - (2025 春招冲刺)

    [x] 核心痛点与混合智能架构定义

    [x] Gazebo + ROS 基础仿真环境搭建

    [ ] (开发中) RL (PPO) 动态避障模块

    [ ] (开发中) Google OR-Tools 离线求解器 API 封装

    [ ] (待办) CV (YOLOv8) 动态障碍物识别 API

    [ ] (待办) 最终 A/B Test 数据对比与分析


## 6. 如何运行 (Getting Started)

本项目完全基于 Docker Compose 构建，旨在实现“一键复现”。
