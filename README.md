# Project Management Dashboard

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiNTcyZDkzZGQtZTFjMi00N2VlLWJmMDEtMGE3MmM5MGViOTdkIiwidCI6ImY0Yzk4NzdkLTM2MDctNGZlOS05OGQzLTkzYzNkMmEwYzgwZCJ9

## Problem Statement

In the current project management infrastructure, stakeholders facing a critical gap in the absence of a dedicated project monitoring tool. As the projects grow in complexity and scale, it has become increasingly challenging to effectively track progress, manage resources, and ensure timely delivery without a centralized monitoring mechanism.
The lack of a project monitoring tool has a significant impact on project delivery, team productivity, and overall organizational efficiency:

-	Increased project management overhead.
-	Reduced transparency and accountability.
-	Higher risk of project delays and budget overruns.
-	Lower stakeholder satisfaction and confidence in project outcomes.

## Dashboard Overview
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/082828f8-e212-4460-825d-be0d2c9ae3c3)

## Data Sources and Integration 

- **Data Sources:**  Excel File,Primavera, Microsoft Project, or similar tools.

- **Data Transformation:** Extracted data is cleaned and transformed to ensure consistency and accuracy. This involves data cleaning (removing duplicates, handling missing values) and data transformation (aggregating data, calculating metrics).
- **Data Loading:** Transformed data is loaded into a centralized database or directly into Power BI using Power Query. This step may involve setting up data flows and refresh schedules to ensure up-to-date information.
- **Data Integration:** Data from different sources is integrated within Power BI to create a cohesive dataset. Relationships between different data tables are established to allow for comprehensive analysis and reporting.

## Key Metrics and KPIs
- **Metrics :** Overall progress (planned vs. actual), financial expenditure, safety metrics.

- **KPIs:** Project milestones, critical issues, top 5 risks, Open NCRs etc.

## Project Components
 1. **Portfolio** :
       ![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/ba81abcf-3106-470a-b795-cd457e7e1912)
       This will display the key summary including the project progress for a particular project. There will be a map to display the project location.

- To showing Project Description in a box for a Seclected Project .


        Project_Description = "Project Description:   "&SELECTEDVALUE('Project summary'[Project Description],"")


2. **Dashboard** :
     ![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/7cc59375-e4c7-420c-9487-2a67471881ee)

     A project performance is a detailed description of a project's goals and objectives, the steps to achieve these goals, and the expected outcomes. In addition, a project overview enables you to outline the project schedule, budget, necessary resources, and status.


3. **Physical Progress** :
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/38218f8b-c280-4e6c-a3ad-1967e8bc668f)

"Physical progress" typically refers to the tangible or measurable advancement made in completing the physical aspects of a project. It encompasses the actual work done on-site, construction activities, manufacturing processes, or any other physical tasks directly related to the project's objectives.

![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/6427ec22-f268-40d3-99dc-70722bf3d08a)
-To showing Planned VS Actual on S-Curve, measure I created.

        S_T_Cumulative_Actual = Var Weigintoplan=SUMX('D&F_Phy&_Func progress','D&F_Phy&_Func progress'[Weightage]*[S_Curve_Cum_Actual])
        Var weightages=SUM('D&F_Phy&_Func progress'[Weightage])
        var plan_start=MIN('D&F_Phy&_Func progress'[Planned Start Date])
        Var Plan_Finish=IF(MAX('Actual Progress'[Progress Date])>MAX('D&F_Phy&_Func progress'[Planned Finish Date]),MAX('Actual Progress'[Progress Date]),MAX('D&F_Phy&_Func progress'[Planned Finish Date]))
        Var t1=Min('Project Calendar'[Date])
        Var t2=Max('Project Calendar'[Date])
        Var t1_st=IF(AND(plan_start<=t1,t1<=Plan_Finish),TRUE(),FALSE())
        Var t2_st=IF(AND(plan_start<=t2,t2<=Plan_Finish),TRUE(),FALSE())
        Var Not_Blank=DIVIDE(Weigintoplan,weightages)
        Return
        IF(OR(t1_st,t2_st),Not_Blank,BLANK())

-----------------------------------------------

        S_T_Cumulative_Planned = 
        Var Weigintoplan=SUMX('D&F_Phy&_Func progress','D&F_Phy&_Func progress'[Weightage]*[S_Curve_Cum_Planned])
        Var weightages=SUM('D&F_Phy&_Func progress'[Weightage])
        Var Cu_Pl=DIVIDE(Weigintoplan,weightages)
        var plan_start=MIN('D&F_Phy&_Func progress'[Planned Start Date])
        Var Plan_Finish=IF(MAX('Actual Progress'[Progress Date])>MAX('D&F_Phy&_Func progress'[Planned Finish Date]),MAX('Actual Progress'[Progress Date]),MAX('D&F_Phy&_Func progress'[Planned Finish Date]))
        Var t1=Min('Project Calendar'[Date])
        Var t2=Max('Project Calendar'[Date])
        Var t1_st=IF(AND(plan_start<=t1,t1<=Plan_Finish),TRUE(),FALSE())
        Var t2_st=IF(AND(plan_start<=t2,t2<=Plan_Finish),TRUE(),FALSE())
        RETURN
        IF(OR(t1_st,t2_st),Cu_Pl,BLANK())




4. **Financial Progress** :
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/df84dac7-61ac-4ef0-9684-f4941392e5ae)

Financial progress in a project refers to the tracking and evaluation of financial aspects related to the project's budget, expenses, revenue, and financial performance. Monitoring financial progress is crucial for ensuring that the project remains within budgetary constraints, optimizing resource allocation, and achieving financial objectives.


5. **Project Milestone Status** :
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/52c1a210-1862-4d6d-847e-a6f520a5ff1e)

Financial progress in a project refers to the tracking and evaluation of financial aspects related to the project's budget, expenses, revenue, and financial performance. Monitoring financial progress is crucial for ensuring that the project remains within budgetary constraints, optimizing resource allocation, and achieving financial objectives.

-For calculating Plan% and Actual% Measure I Created

        Plan_Per = 
        Var dif=DIVIDE(DATEDIFF(MAX('Project summary'[Baseline Start]),[Selected Date],DAY),DATEDIFF(MAX('Project summary'[Baseline Start]),Max('Project Milestones'[Plan finish]),DAY),0)
        RETURN
        IF(dif>1,1,dif)
----

        Actual_Per = 
        Var dif=DIVIDE(DATEDIFF(MAX('Project summary'[Baseline Start]),[Selected Date],DAY),DATEDIFF(MAX('Project summary'[Baseline Start]),MAX('Project Milestones'[Forecast finish]),DAY),0)
        RETURN
        IF(OR(NOT(ISBLANK(MAX('Project Milestones'[Actual finish]))),dif>1),1,dif)


6. **Project Critical Issues**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/9cde7d4e-dbbb-4f7d-b571-eb0a5f641a87)

This dashboard will contain the red flag area of a project. There will be an issue log and details of impact caused by the issue. On the other hand, it will also display the requested action and the status of the risk with timeline.

7. **Risk Management**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/eaa2d05b-4cca-494f-bba3-f2610e84a4a7)

The Key Risk is a component of the Power BI dashboard, designed to highlight potential issues and areas of concern within a project. It incorporates a table visualization sourced from an Excel file, providing a structured overview of red flag incidents.


8. **Quality Performance**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/f17adb33-55ef-4a9c-8229-7c44bd6eb4a5)

The Quality Performance dashboard is an essential part of the Power BI dashboard, dedicated to providing insights into the project's quality parameters. It includes various visualization fields sourced from an Excel file, represented in a table format. The table presents a structured and comprehensive view of the quality performance metrics.


9. **Safety Performance**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/b2d9cad7-600d-4718-aff9-d73edad40203)

The Safety Performance dashboard is a crucial part of the Power BI dashboard, dedicated to providing insights into the project's safety parameters. It includes various visualization fields sourced from an Excel file, represented in a table format. The table presents a structured and comprehensive view of the safety performance metrics.

10. **Approval & Enablers**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/7c6eb8b0-1ad2-406b-94c9-a8b64e5fe387)

The Project Enabler Tracker dashboard is an integral part of the Power BI dashboard, which has three components as Condition Precedent Tracker, Land Acquisition Tracker & Environment Clearance Tracker.

11. **Engineering Tracker**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/97fc0858-558a-414f-a60a-4c127f1544ca)

The Engineering Deliverable Tracker dashboard is a crucial part of the Power BI dashboard, designed to monitor and manage engineering deliverables for a project. It incorporates a table visualization sourced from an Excel file, presenting essential information related to each engineering deliverable.

12.	**Procurement Tracker**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/9a8a99ef-ee03-4826-b954-2be0fd834b24)

The Procurement Deliverable Tracker dashboard is a vital part of the Power BI dashboard, designed to monitor and manage procurement deliverables for a project. It incorporates a table visualization sourced from an Excel file, presenting essential information related to each procurement deliverable.

13.	**Construction Tracker**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/a6be9d07-ea70-4e20-a0f5-8c5d219e41be)
The Construction Tracker dashboard is a critical part of the Power BI dashboard, designed to monitor and manage construction activities for a project. It incorporates a table visualization sourced from an Excel file, presenting essential information related to each construction activity.

14.	**Package Performance**:
![image](https://github.com/AlamKhan8140/Alam_portfoli/assets/156282145/3ad7046c-f408-4744-aa09-332f3e2ad59f)

The Construction Tracker dashboard is a critical part of the Power BI dashboard, designed to monitor and manage construction activities for a project. It incorporates a table visualization sourced from an Excel file, presenting essential information related to each construction activity.
