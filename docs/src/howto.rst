.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. note:: These instructions are only for the Amazon-internal preview of the |cdk|.

.. _how_to:

######################################
Performing Common Tasks With the |cdk|
######################################

This topic describes how to perform some common tasks with the |cdk|.
Each section starts with the question "How do I" and supplies an answer.
Note that there are often multiple ways of performing a task.
This topic describes the simplest, most straightforward way.

.. _how_to_use_cfn_template:

How do I use an Existing |CFN| Template?
========================================

The |cdk| provides a mechanism that you can use in your code to include an
existing |CFN| template in your app.

The following example includes the template *my-template.json* into the
existing app and gets the ARN of the bucket **mybucket** from the
template.

.. code-block:: js

   import { FnGetAtt } from '@aws-cdk/core'
   import { readFileSync } from 'fs'

   new Include(this, 'ExistingInfra', {
        template: JSON.parse(readFileSync('my-template.json').toString))
   });

   // To get the bucket's ARN:
   const bucketArn = new FnGetAtt('mybucket', 'Arn');

The *my-template.json' must have the following resource,
where **abcdwxyz** is the unique, 8-character hash that the |cdk| generates for the resource:

.. code-block:: json

   "TheBucketabcdwxyz": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
         "BucketName": "mybucket"
      }
   }
