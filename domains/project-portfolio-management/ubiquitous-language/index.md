# Project Portfolio Management DDD - Ubiquitous Language

Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as a Portfolio with its Projects.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts.
Backlog: A prioritized list of work items, features, or requirements waiting to be scheduled and executed.
Baseline: An approved snapshot of a project plan, budget, or schedule used as a reference point for measuring variance.
Benefit: A measurable positive outcome expected from a project or program, such as revenue increase or cost reduction.
Bounded Context: A well-defined boundary within which a particular domain model applies and terms have specific meaning.
Budget: The approved financial plan for a project or portfolio, including planned costs across categories and time periods.
Business Case: A documented justification for a proposed project, including expected costs, benefits, risks, and strategic alignment.
Capacity: The total amount of work that a resource or team can handle during a given time period.
Change Request: A formal proposal to modify a project's scope, schedule, budget, or deliverables.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Core Domain: The most critical and complex subdomain that provides competitive advantage and requires the most investment.
Cost Variance: The difference between the budgeted cost and the actual cost of work performed.
Critical Path: The longest sequence of dependent tasks that determines the minimum project duration.
Deliverable: A tangible or intangible output produced as a result of project work, subject to acceptance criteria.
Dependency: A relationship between tasks or projects where one must complete before another can begin.
Domain Event: A record of something significant that happened in the domain, such as ProjectApproved or BudgetExceeded.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Earned Value: A measure of work performed expressed in terms of the budget authorized for that work.
Entity: A domain object defined by its unique identity that persists over time, such as Project, Portfolio, or Resource.
Forecast: A prediction of future project or portfolio performance based on current trends and remaining work.
Gantt Chart: A visual representation of a project schedule showing tasks, durations, dependencies, and progress over time.
Gate Review: A formal decision point in a project lifecycle where continuation, modification, or termination is determined.
Generic Subdomain: A subdomain that is not specific to the organization and can use off-the-shelf solutions.
Investment Category: A classification of projects by strategic theme, such as growth, maintenance, compliance, or innovation.
Issue: A current problem or concern that could impact project delivery and requires resolution.
KPI (Key Performance Indicator): A measurable value that demonstrates how effectively a portfolio or project is achieving its objectives.
Milestone: A significant point or event in a project timeline, typically marking the completion of a major phase or deliverable.
Portfolio: A collection of projects, programs, and initiatives managed together to achieve strategic business objectives.
Priority: A ranking that determines the relative importance and sequencing of projects or tasks within a portfolio.
Program: A group of related projects managed in a coordinated manner to obtain benefits not available from managing them individually.
Project: A temporary endeavor undertaken to create a unique product, service, or result with defined scope, schedule, and budget.
RAG Status: A Red-Amber-Green indicator summarizing the overall health of a project across schedule, budget, and scope dimensions.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
Resource: A person, team, equipment, or facility required to execute project work.
Resource Allocation: The assignment of available resources to specific projects or tasks based on priority and capacity.
Risk: An uncertain event or condition that, if it occurs, has a positive or negative effect on project objectives.
Risk Appetite: The level of risk an organization is willing to accept in pursuit of its strategic objectives.
Risk Mitigation: A strategy to reduce the probability or impact of an identified risk.
ROI (Return on Investment): A financial metric measuring the profitability of an investment relative to its cost.
Schedule Variance: The difference between the planned progress and the actual progress of a project.
Scope: The totality of work that must be performed to deliver a project's outputs and achieve its objectives.
Scoring Model: A quantitative framework used to evaluate and rank projects based on weighted criteria.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
Sponsor: The individual or group that provides resources and support for a project and is accountable for its success.
Stakeholder: Any person or organization that is actively involved in or may be affected by a project or portfolio decision.
Strategic Alignment: The degree to which a project or portfolio supports the organization's strategic goals and objectives.
Supporting Subdomain: A subdomain that is necessary for the business but not a core differentiator.
Task: A discrete unit of work within a project, with defined inputs, outputs, duration, and resource requirements.
Value Object: An immutable domain object defined by its attributes rather than identity, such as CurrencyAmount, DateRange, or Priority.
WBS (Work Breakdown Structure): A hierarchical decomposition of the total scope of work into manageable work packages.
