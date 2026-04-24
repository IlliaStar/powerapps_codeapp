---
name: "Deploy"
description: Build and deploy the Power Apps code app to the configured environment
---

Deploy the Power Apps code app to the Power Platform environment defined in `power.config.json`.

**Steps**

1. **Build the app**
   Run `npm run build` and report any TypeScript or build errors. Stop if the build fails.

2. **Push to Power Platform**
   Run `pac code push` and report the result.
   - On success: show the play URL from the output
   - On auth error (`AADSTS` / token expired): tell the user to run `pac auth create --deviceCode` and enter the code at https://login.microsoft.com/device, then retry
   - On `CodeAppOperationNotAllowedInEnvironment`: tell the user to enable "Power Apps code apps (preview)" in the Power Platform admin center for environment `$ENVIRONMENT_ID` from `power.config.json`

3. **Report**
   Show a summary:
   - Environment: the `appDisplayName` and `environmentId` from `power.config.json`
   - App URL: the play link returned by `pac code push`
   - Build size: the output sizes from the Vite build step
