---
layout: post
title: "Make Compliance Great Again"
date: 2023-08-12 00:00:00
description: Addressing common issues I see in compliance projects
tags: Security
---

From what I see, compliance people and auditors seem to have a bad reputation among security practitioners, especially among those who hold more technical roles in the industry such as security researchers, security operations people, and software engineers (especially those who mainly work on systems with high security requirements).

In 2022, the Indonesian government and state-owned enterprises suffered a series of large-scale data breaches. Many of those organizations are ISO 27001-compliant and they denied the breaches by bringing up their ISO 27001 compliance despite the circulating evidence proving otherwise.

One of the more technical Indonesian security practitioners whom I know from Twitter responded to the series of data breaches with something along the line of "lol, ISO 27001" as he expressed his distrust towards the compliance standards.

One of the arguments I've heard, and been guilty of raising myself, is how having a compliance certification to a certain security standard doesn't make your system secure, only building and running it correctly do. This is actually correct, and security standards as listed in ISO 27001 and PCI-DSS compliance checklists are usually stated in a way that's quite vague, open to various interpretations, and may not provide much insight on how it should be implemented.

This leads to many possible ways to implement the system requirements demanded by the security standard, and the consultants and auditors might not have deep or wide enough expertise regarding the fields related to the security context of the system they're advising to be able to guide the clients towards the stronger version of the implementation.

In this article, I'd like to share a bit about what I think great compliance people, IT auditors, and security consultants should avoid in order to better guide their clients toward implementing more secure systems that comply with the standards they're trying to push.

# Championing Inefficient Manual Processes

Compliance people's main focus is usually directed at fulfilling a set of compliance checklists to ensure the system fulfills the requirements of the compliance standards they're aiming to be compliant with. This is literally their job, and there's nothing wrong with that.

But the methods they push for their clients to comply with the standards might not be the most optimal approach for the client, and it might be inefficient enough that the client ignores it in their day-to-day operations and only treats it as a formality to get their compliance certificate.

One example I've seen is a consultant who assisted a small company to achieve ISO 27001 compliance. One of the requirements of ISO 27001 is to make sure every access granted to employees follows a proper approval process. The access needs to be requested by the employee, then approved by the employee's direct supervisor, and finally reviewed and approved by the system administrator for the access to be provisioned. The consultant pushed their client to implement this by using paper forms to document the access requests, which need to be signed by the supervisors and the system administrators as proof that a proper process is followed in the access provisioning flow.

While the process might be reasonable to be implemented when the company is still very small, it becomes a paperwork nightmare and it hinders the company's ability to scale the operations as the company's headcount grows. The company management tries to keep the process as proper as possible according to the guidance given by the consultant they hired early on when implementing ISO 27001 standards in their company. But at this point, it's too much of a hindrance for the employees' work, and the employees eventually started ignoring the proper approval process in order to make their work easier and more efficient.

In this case, the consultant failed to educate their client that the core point of the requirement is to have a proper approval line and well-recorded history of the provisioned accesses within the system. The client is stuck with a suboptimal process taught to them by the consultant and is afraid of changing the process without any consultant telling them that it's okay to introduce changes to the implementation of that requirement because they don't want to risk losing their compliance certification.

A great consultant should be able to educate the client that it's the control that matters, not the physical papers with signatures on it. The client should always be able to improve how they fulfill the requirements as they see fit, as long as the required controls are in place and the records are kept for auditability. This way, the clients can understand that it's okay for them to switch from paper-based approval flow to using JIRA tickets-based approval flow to improve the effectiveness of their operations, or even develop their own custom software that could be better integrated into their business process and IT system.

The lack of understanding from the client's side might lead the client to cling onto a workflow that eventually turned into a security threat for their organization due to its inefficiency making people ignore it on a day-to-day basis, which in turn opens a gap in the client's security posture for an attacker to exploit.

# Being Too Paperwork-Oriented

Compliance people require evidence and records to prove that the organization runs its process correctly, according to the standards they're committed to complying with. These evidence and records are usually required in the form of documents and paper records to be shown to auditors whenever needed.

This is understandable, given how legalities work. But sometimes compliance people do request for the paperwork to be done in a certain way that might not be realistic to be performed in a real scenario. For example, let's suppose that a compliance officer is requesting evidence that their IT team has a proper incident response process.

In this scenario, the compliance officer specifically requests for the incident response life cycle to follow a process they consider proper from a compliance point of view. The steps are:

- The incident reporter must fill out a form detailing the incident they witnessed along with the details.
- The form must be passed to the SOC team for initial review to see if it really is an incident and it must get the SOC staff's sign-off if it's a real incident, it can be marked as a false alarm if it turns out to be one.
- The form must be passed to the incident response manager for escalation to the stakeholders and coordination with the incident handling.
- Throughout the investigation and incident response process, every activity must be documented in the form by the personnel who works on the said activity.
- When the incident response process is done and the problem is solved, state it in the form and have it signed by senior management personnel.

The compliance officer demands that everything must be filled properly by the person responsible for each step, including the time of the reporting.

While this is ideal from a compliance standpoint, this flow doesn't hold in real-life incident scenarios. The reasons for this are:

- The incident reporter might not be aware of the technicalities of the incident they reported, the reporter might not even be technical at all. And if it's an emergency, they probably have more urgent things to do than filling a form with detailed information about the incident and signing it to be passed to the SOC team. From my experience, they usually make an emergency call with a short explanation about the problem and ask us to come check it out.
- Rather than reviewing a written form, the SOC team will be better off talking directly to the incident reporter while the incident reporter shows them what actually happened and how. This will help ensure the accuracy of the information received by the SOC team while allowing the incident reporter to communicate with the SOC team more quickly and effectively.
- After the incident is escalated to the incident response manager and the stakeholders, we will start with an investigation and containment of the incident. This process can take hours, and for incidents that require complex investigation, it might take up to a few days of work (and overtime work). While at this stage the team will be taking notes of the investigation progress and containment process, it's unreasonable to expect them to write the final report while working on it especially if they still haven't fully figured it out. The complete post-mortem report is usually written after the containment is done.
- After the containment, we need to plan for further mitigation and system improvements as needed to avoid the same problem in the future. At this point, we have only applied a band-aid to temporarily fix the problem. The proper fix can take any time from days to months to implement, depending on the complexity of the issue at hand.

Hence, the more reasonable approach for this process might be to just keep track of the approximate time of reporting, analysis, escalation, and the rest of the process as a quick note while working on it. The notes will later be compiled as a report only after the incidents are properly handled to make sure the cognitive load of the incident responders is focused on investigating and containing the incident first.

Being too strict with the properness of the form-filling workflow can end up hindering the effectiveness of the work in the organization. While having the paper evidence is a requirement for the compliance officer in this scenario to do their job, with more flexibility they can get the same report without adding too much unnecessary overhead to the team's work.

# Lack of Technical Understanding

Understandably, compliance people aren't required to be as technical as software engineers, system administrators, and security engineers. But in some cases, the lack of technical understanding becomes a problem for compliance people when communicating with people holding the more technical roles in the organization.

Suppose a compliance officer is planning for the IT system in their whole organization to apply a password standard with a minimum password length of 12 characters and a combination that must consist of upper-case, lower-case, numeric, and special characters. As of the time I'm writing this article, these password requirements are usually considered the best practice by compliance people and auditors.

The compliance officer then gathered the engineers to explain what they'd like the engineers to implement in the systems under their scope of ownership. Some of the systems that previously use 8 characters as the minimum length for their password policy will see an improvement in their password security just by increasing the minimum password length to 12 characters.

But some systems don't use passwords for authenticating their users:

- Some systems use mTLS for authentication.
- For SSH, the system administrators set up the servers to only allow authentication by using SSH key pairs or SSH certificates.
- Some systems require a time-based one-time password synchronized with the user's smartphone in order to authenticate instead of the traditional password mechanism.

These approaches are generally considered to be more secure than using a traditional password. But sometimes the compliance officer still demands the traditional password approach to be implemented in these systems in order to make it more secure.

While it is indeed more secure due to the added factor of authentication that contributes as another layer of security, the existing factor is already generally accepted to be more secure than the factor they're asking to be added in this case. So while adding it might still improve the security, it will worsen the user experience for the people who need to use these systems while incurring more development costs for the engineers to implement a password as an authentication factor in the system that doesn't really require it.

# Unclear Scope Definition

In a compliance project, one of the most commonly requested pieces of evidence is the evidence of user activity logs in the system in order to audit the activities of the users within the system. I notice that the scope of the activity log collection is one of the parts that are not usually very well-defined by the compliance project lead, which later turns into confusion when determining which system's logs are required to be collected and how detailed it needs to be.

This might be due to proper production-grade systems generally having logging capabilities for user activities, at least to some extent. But because practically everything has a user activity logging feature, it's easy for the compliance officers to misunderstand which exact system's logs they need to collect as evidence and how detailed they expect the logging should reasonably be.

Suppose the goal of the project is ISO 27001 compliance for a business unit in the company that's supported by consumer-facing SaaS software and a back-office system for company operations. A compliance officer is assigned to reach out to the IT team to collect evidence of compliance regarding user activity logging in the system, where the scope of the audit is primarily the application. The compliance officer demands database activity logs of employees in the system who have the privilege to make changes to the database records.

This request has a few problems:

- The database records are primarily updated by employees via the back-office system.
- The back-office system is connected to the database by using one database credential for the back-office application to open the database connection.
- Hence, every activity performed by the back-office system to the database will be recorded in the database logs as activities performed by a single database user.

What the compliance officer should request is evidence of user activity logs in the back-office system's application software. The expected log should be the log of the employee who performed a certain action on a certain object in the system, which is represented by a database record.

This should be quite obvious to technical people, but might not be as obvious to the compliance officer especially if they're not very technical. But failing to communicate this correctly could cause confusion along with a lot of communication overheads and wasted time when coordinating the evidence collection with the technical team.

An even worse outcome is if the compliance team makes the technical team implement a one DB user per employee policy and makes the application use those DB users to access the DB on behalf of the employees performing the action, which can make the implementation extremely complicated, messy, and expensive to implement and maintain (which makes it potentially even less secure due to the complexity). While I don't think this is a common outcome, I wouldn't say that it is impossible to happen in an organization where the technical people aren't context-aware enough to argue with the compliance people, and the compliance people have the final say in the decision.

In this case, it's ideal for the compliance personnel to have at least general knowledge in software and systems architecture, while the technical people also should have a good understanding of the scope of the system that should comply with the standards and what should be expected from the system.

# Conclusion

Compliance standards are made to help guide organizations to implement security best practices within their scope of compliance. Due to how the standards are defined and how they should be implemented, it's ideal for the compliance people and auditors to be able to see things from both a legal-like point of view and a technical point of view.

In reality, most people who are drawn to compliance work tend to be stronger on the legal-like aspects of compliance than the technical aspects of it. People who’re stronger on the technical side might have a stronger preference for technical design and implementation work while having little interest in compliance. This leads to a gap between how compliance people see things and how technical people see things, which might cause issues when organizations are trying to implement the standards in their context. The gap needs to be minimized to enable both the compliance and the technical people to work together towards a better solution that helps the organization comply with the standards they're implementing while being actually secure and efficient.

The technical people tend to not understand the standards as well as the compliance people, so they need to be advised and they need to proactively provide context to the compliance people while being open to understanding the reason why the standards are defined that way in order to contribute effectively to the project. Meanwhile, the compliance people tend to not understand the underlying technical principles governing the system's design and implementation decisions as well, which might lead to unrealistic expectations and suboptimal solutions proposed by compliance.

While in real life I've met people who're excellent in both compliance knowledge and technical capabilities, they're not as common as I wish them to be. And while both compliance and technical people are responsible for the quality of the solution implemented in the organization, I put more weight on the compliance people as the ones responsible for guiding the organization toward implementing better security, especially since more compliance people work as consultants to help inexperienced organizations implement their systems according to the security standards than technical people.
