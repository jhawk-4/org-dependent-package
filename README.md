# Salesforce Troubleshooting

## Steps to reproduce

  1. Obtain a Duv Hub Trial Org
  2. Clone this repo
  3. Authenticate the Dev Hub
  4. Authenticate a sandbox org (optional)
  5. Install AgileAccelerator managed package by executing `$sfdx force:package:install -p "Salesforce Agile Accelerator" -w 10`
  6. Create an org dependent unlocked package by executing `$sfdx force:package:create -n OrgDependentTestPkg -d "Test package for Salesforce troubleshooting" -t Unlocked -e -r force-app --orgdependent`
  7. Create a version of the package just created by executing `$sfdx force:package:version:create -p OrgDependentTestPkg -x -w 10`
  8. Promote the package for install in a production org by executing `$sfdx force:package:version:promote -p OrgDependentTestPkg@0.1.0-1 -n`
    
  - Note: This step is only needed if you are planning to install the package in a production instance.  If you are using a sandbox, you can skip this step
  9. Install package in destination org by executing `$sfdx force:package:install -p OrgDependentTestPkg@0.1.0-1 -w 20`

## Expected Outcome
The expected outcome is that the package install will complete in the destination environment.

## Acutal Outcome
The addition of a workflow rule on a managed package object causes the package install to fail with the following message:

>Cannot add component of type:CustomObject named:agf__ADM_Work__c subjectId:01I3t000000IqTh to another package because it is an installed component., Details: package.xml: Cannot add component of type:CustomObject named:agf__ADM_Work__c subjectId:01I3t000000IqTh to another package because it is an installed component.

If the agf__ADM_Work__c.workflow-meta.xml file is removed from the source and steps 7-9 are repeated, the expected outcome is achieved.  
