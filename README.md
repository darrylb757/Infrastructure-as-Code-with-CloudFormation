# Infrastructure-as-Code-with-CloudFormation
I will demonstrate how to set up a cloud formation template. I'm doing this project to learn about IaC and how I can do this to my own architecture, ie my CI/CD architecture. 
<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Infrastructure as Code with CloudFormation

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-cloudformation-updated)

**Author:** Darryl Brown  
**Email:** darrylbrown1991@gmail.com

---

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

## Introducing Today's Project!

In this project, I will demonstrate how to set up a cloud formation template. I'm doing this project to learn about IaC and how I can do this to my own architecture, ie my CI/CD architecture. 

### Key tools and concepts

Services I used were Cloud Formation, Code Artifact, Code suite of services. Key concepts I learnt include Infrastructure as Code, how to use the IaC generator, resolving circular dependencies, resources that might depend on other resources to be created first. I also learned how to write my own resources definitions. 

### Project reflection

This project took me approximately 1 1/2 hrs to complete. The most challenging part was editing my Cloud Formation template. It was most rewarding to deploy my own template and using Infrastructure as Code.

This project is part six of a series of my step by step how I complete my DevOps projects where I'm building a CI/CD pipeline!

---

## Generating a CloudFormation Template

The IaC Generator it scans like taking inventory of everything in your AWS account that could be part of your CloudFormation template. It's essential because the IaC Generator needs to discover what you have before it can create a template for it. It works in a three-step process where. It scans all the resources in AWS account. Creates a template, which is a bundle of scanned resources that you'd like to deploy and manage together. It imports the template into CloudFormation to deploy the resources in one go.

A CloudFormation template is a bundle of scanned resources that you'd like to deploy and manage together. The resources that I could add to my template include role, policies, instance, groups, code artifacts.  

The resources I couldn’t add to my template were CodeBuild project and CodeDeploy deployment group are great examples of this. Because they require specific configuration details and security permissions that our generator isn't able to handle automatically.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_0495b046)

---

## Template Testing

Before testing my template, I delete existing resources and test out my cloud formation template because if I try to deploy with same resources already present it will fail.

I tested my template by create a stack in CloudFormation with the template i created. The result of my first test was a failure because while CloudFormation is in the process of creating my IAM role, it's also trying to create and attach policies to this role at the same time. It's like trying to find a file in a folder before I've actually saved it there

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_f56730fd)

---

## DependsOn

To resolve the error, I added a DependsOn line in my template via VScode. The DependsOn attribute means I'm telling CloudFormation to create the IAM role first before creating the policies.

The DependsOn line was added to four different parts of my template: CodeArtifact policy, CodeBuild policy, CloudWatch policy, Code Connection poliy. For CodeArtifact policy I also told it to wait for EC2 role to attached and created.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_f0df8018)

---

## Circular Dependencies

I gave my CloudFormation template another test! But this time i received an error for circular references.

To fix this error, I located ManagedPolicyArns under my IAM role for CodeBuild and delete the lines referencing that role in my Cloud Formation template. The Infrastructure as Code generator helps by grabbing the current setup of my AWS resources, but it doesn't rearrange them to dodge issues like circular dependencies.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_e6fd85ed)

---

## Manual Additions

In a project extension, I manually defined two more resources: CodeBuild Porject and CodeDeploy Deployment Group.

I also had to make sure the references were consistent in this template, so I edited resource IDs in my additions to match the actual resources in my template.

I also introduced Parameters, which can make my CloudFormation template reusable and flexible. Instead of hard coding values like GitHub usernames, I can parameterize them. This lets me use the same template for different environments or repositories without modifying the template itself.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_1cee0428)

---

## Success!

I could verify all the deployed resources by visiting the resource tab in CloudFormation > Stack> Resources.

![Image](http://learn.nextwork.org/sincere_vermilion_playful_beaver/uploads/aws-devops-cloudformation-updated_bd8b836b)

---

---
