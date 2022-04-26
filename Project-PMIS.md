---
title: PM指南:项目管理信息系统
date: 2019-04-06 00:32:40
tags: [工具癖,Manager,Engineering]
categories: 工程技术
---
## 摘要

>信息化带来的价值可以占到总投资的10%以上。换句话说，一个工程项目如果管理信息化工作成功了，可以节省十分之一的投资，这是一个非常可观的数据。所以如何做好工程管理信息化，如何将信息资源开发好，将信息技术利用好，对现在乃至将来的工程建设有着重大的意义。—— 丁士昭

<!--more-->

- 项目管理信息系统，PMIS--Project Management Information System

## 任务管理

- [《凤凰项目》| 从作坊到工厂的寓言故事](https://riboseyim.com/2018/04/10/DevOps-Phoenix/)
- [PM指南:软件业看板Kanban管理实践|工具与技术](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)
- [PM指南:开源项目管理平台Redmine|工具与技术](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)

## 资源管理
- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)
- [DevOps 实践的本质是文化](https://riboseyim.github.io/2018/03/29/DevOps-Culture/)

## 行业应用案例

#### DevOps：从供应链管理到软件研发管理

- [Kanban 看板管理实践](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)
- [《凤凰项目》：从作坊到工厂的寓言故事](https://riboseyim.github.io/2018/04/10/DevOps-Phoenix/)


## Free Project Management Tools (Single User & Startup Company)

>Free is not free

#### Overview of Free Project Management Tools

| Name          | focus                    | free version       | company version | UI        |
| ------------- | ------------------------ | ------------------ | --------------- | --------- |
| Asana         | tracking everything      | 15 members         | yes             | beautiful |
| Paper         | real-time reporting      |                    |                 |           |
| Trello        | Kanban system            | 10MB  attach files | yes             |           |
| PushMon       | monitor cronjobs scripts | 3 URLs, 4 credits  | yes             |           |
| Teamweek      | manage schedules         | 5 members          | yes             |           |
| ClickUp       | solution                 | all features       | no              |           |
| Wrike         |                          |                    |                 |           |
| OpenProject   |                          |                    |                 |           |
| Gantt Project | gantt                    |                    |                 |           |
| MelsterTask   |                          |                    |                 |           |
| KanbanFlow    | Kanban board             |                    |                 |           |
| Labourhood    |                          |                    |                 |           |
| Kanban Tool   | Kanban board             |                    |                 |           |
| Redmine       | professional             | open-sourced       | no              | normal    |
| Airtable      |                          |                    |                 |           |
| Barvas        |                          |                    |                 |           |
| actiTIME      |                          |                    |                 |           |

#### Asana

- focus：tracking everything
- License：15 members
- Cost：free

![Asana - Project Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Asana-Project-Management-Tool.png)

#### Paper

- focus：created by Dropbox;capture, organize, and prioritize issues, plan sprints, and take advantage of real-time reporting.
- License：
- Cost：free

![Dropbox Paper - Collaborative Workspace](https://www.fossmint.com/wp-content/uploads/2018/10/Dropbox-Paper.png)

#### Trello

- focus: It implements the Kanban system
- License：free & company
- Cost：free version can add 10MB of files from your computer or link any file in Google Drive, Box, OneDrive, and Dropbox accounts and you can add a max of 1 power-up per board.

![Trello - Project Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Trello-Project-Management-Tool.png)


#### [PushMon](https://www.pushmon.com)

- focus: not a typical project management tool;It is used to **monitor scripts, cronjobs, and scheduled tasks** and get notifications directly to your email, mobile phone, etc.
- License：free & company
- Cost：The free version of PusMon gives you access to 3 URLs, 4 credits, and instant notification alerts via email, SMS, Twitter, IFTTT, phone calls, etc.

```java
package com.teamextension.ping;

import java.io.InputStream;
import java.net.URL;
import java.net.URLConnection;

public class UrlPing {
	public static void main(String[] args) {
		String urlString = "http://pshmn.com/eaFnY";
		pingUrl(urlString);
	}

	private static void pingUrl(String urlString) {
		try {
			URLConnection conn = new URL(urlString).openConnection();
			conn.setConnectTimeout(5000);
			conn.setReadTimeout(5000);
			InputStream is = conn.getInputStream();
			is.close();
		} catch (Exception e) {
			// log error
		}
	}
}
```

#### Teamweek

- focus: track of deadlines in calendar form, manage schedules, create Gantt charts.
- License：free & company
- Cost：The free version allows a maximum of 5 team members.

![Teamweek - Project Management Software](https://www.fossmint.com/wp-content/uploads/2018/10/Teamweek-Project-Management-Software.png)


#### ClickUp

- focus: project management solution.
- License：free & company
- Cost：The free version gives you access to unlimited users, tasks, and projects.


![ClickUp Sorting and Filtering](https://www.fossmint.com/wp-content/uploads/2018/08/ClickUp-Sorting-and-Filtering.png)

#### Wrike

- focus: simplify your project plans, streamline your workflow, and enable collaboration.
- License：free & company
- Cost：The free version allows a maximum of users in a team and you can use a simple shared task list for your projects.

![Wrike - Project Management Software](https://www.fossmint.com/wp-content/uploads/2018/10/Wrike.png)

#### OpenProject

- focus: simplify your project plans, streamline your workflow, and enable collaboration.
- License：open-source & free & company (Community, Cloud, and Enterprise)
- Cost：The community edition is available for free with features including time management, team collaboration, Gantt charts for project planning, budgeting, and reporting. It also supports Agile for project management with backlogs, roadmaps, bug tracking, etc.

![OpenProject - Collaborative Project Management](https://www.fossmint.com/wp-content/uploads/2018/10/OpenProject-Collaborative-Project-Management.png)

#### Gantt Project

- focus:  a work breakdown structure, draw dependency constraints, PERT charts, etc.
- License：free software; GPL license.
- Cost：java-based ; US$5 ?
- [download gantt project](https://www.ganttproject.biz/download)

![Gantt Project - Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Gantt-Project.jpg)

#### MeisterTask

- focus:  
- License：version contains all the options required for creating unlimited projects and tasks. You can also collaborate on invited friends in real-time.

![MeisterTask - Task Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/MeisterTask.jpg)

#### KanbanFlow

- focus:  a Lean tool for project management
- License：free.

![Kanban Lean Project Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Kanban-Lean-Project-Management-Tool.png)

#### Labourhood

- focus:  Labourhood is an online project management tool that focuses on online collaboration, networking, and security.
- License：free.
- Cost: Labourhood still in **Beta version** which is free to use all you have to do is sign up to create a free account.

![Labourhood Project Management Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Labourhood.jpg)

#### Kanban Tool

- focus:  Kanban Tool is an online ponline Kanban board.
- License：a 14-day free trial you can experiment with.

![Kanban Tool](https://www.fossmint.com/wp-content/uploads/2018/10/Kanban-Tool.png)

#### Redmine

- focus:  professional features.
- License：free & open-source.
- Cost：It is written using the Ruby on Rails framework.
- [PM指南:开源项目管理平台Redmine|工具与技术](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)

![Remine gantt](http://riboseyim-qiniu.riboseyim.com/gantt@2x.png)

![Remine workflow](http://riboseyim-qiniu.riboseyim.com/workflow_example@2x.png)

#### Airtable

- focus:  spreadsheet-database hybrid.
- License：free.
- Cost：It is written using the Ruby on Rails framework.

![Airtable](https://www.fossmint.com/wp-content/uploads/2018/10/Airtable.png)

#### Barvas

- focus:  focuses on improving your workflow and team productivity.
- License：free(a single user account which is limited to a single project).
- Cost： Access to unlimited projects costs $11.70 and the subscription is $5.85 per month.

![Barvas: Project and Task Management Software](https://www.fossmint.com/wp-content/uploads/2018/10/Barvas-Project-and-Task-Management-Software.png)

#### actiTIME

- focus: time tracking and using intelligent methods to analyze data
- License：free for up to 3 users
- Cost： $394.00 USD per year for 5 users ($6.57/month per user).

![actiTIME - Time Tracking & Scope Management Software](https://www.fossmint.com/wp-content/uploads/2018/10/actiTIME-Time-Tracking-Scope-Management-Software.png)

## 拓展阅读

- [项目管理 | Overview of Project Management](https://riboseyim.com/2019/02/06/Project/)
- [PM指南:PMI项目管理知识体系](https://riboseyim.com/2019/04/30/Project-PMP/)
- [PM指南:建筑工程项目管理|行业案例教学](https://riboseyim.com/2019/03/27/Project-Construction/)
- [PM指南:范围管理](#)
- [PM指南:进度管理](#)
- [PM指南:成本管理](#)
- [PM指南:质量管理](#)
- [PM指南:资源管理](https://riboseyim.github.io/2019/03/12/Project-Resources/)
- [PM指南:沟通管理](https://riboseyim.com/2019/02/06/Project-Communications/)
- [PM指南:风险管理](https://riboseyim.github.io/2018/06/05/Project-Risk/)
- [PM指南:采购管理](https://riboseyim.github.io/2019/03/12/Project-Procurement/)
- [PM指南:相关方管理](#)
- [PM指南:网络计划技术|工具与技术](https://riboseyim.com/2019/05/29/Project-Tech-NetworkPlanning/)
- [PM指南:项目管理信息系统|工具与技术](https://riboseyim.com/2019/04/06/Project-PMIS/)
- [PM指南:项目管理开局模板|工具与技术](https://riboseyim.com/2018/06/19/Project-Template/)
- [PM指南:开源项目管理平台Redmine|工具与技术](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)
- [PM指南:软件业看板Kanban管理实践|工具与技术](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)
- [PM指南:源代码版本管理|工具与技术](https://riboseyim.com/2016/05/31/TeamWork-Git/)

#### 技术专题 [ DevOps 漫谈系列 ](https://riboseyim.github.io/2016/07/28/DevOps/)

- [DevOps 漫谈：基础设施部署和配置管理](https://riboseyim.github.io/2018/03/26/DevOps-Deployment/)
- [DevOps 漫谈：Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)

## 参考文献
- [Comparison of project management software](https://en.wikipedia.org/wiki/Comparison_of_project_management_software)
- [17 Best Free Project Management Tools for You | May 7, 2019by Martins D. Okoi](https://www.fossmint.com/best-free-project-management-tools/)
