# Vue CLI

https://cli.vuejs.org

## Installation

Run the following command in PowerShell to install VueCLI:

```
npm install -g @vue/cli
```

## Install yarn

Install yarn by running `npm install --global yarn`

## Create a new Vue project:

```
vue create my-project
```

<div style="page-break-after: always;"></div>

## If following error occurs.

![vue-create-error-1.png](_resources/vue-create-error-1.png)

Solution: run as follow.

![vue-create-error-1-solution.png](_resources/vue-create-error-1-solution.png)

<div style="page-break-after: always;"></div>

## Change to new project directory(`cd <dir>`) and run `yarn serve`

<img src="_resources/finish-vue-create-and-yarn-serve.png" alt="finish-vue-create-and-yarn-serve" width="850"/>

<div style="page-break-after: always;"></div>

## If following eror occurs.

![yarn.ps1-cannot-loaded.png](_resources/yarn.ps1-cannot-loaded.png)

Solve by setting Execution Policies in PowerShell (Need to run PowerShell as **Administrator**):

![powershell-set-execution-policies.png](_resources/powershell-set-execution-policies.png)

<div style="page-break-after: always;"></div>

Newly created Vue App is running at http://localhost:8080.

![vue-app-is-running.png](_resources/vue-app-is-running.png)

