# aws-lambda-node-minimal-boilerplate
Minimal boilerplate for node.js app intended to run on AWS Lambda

No need to clone this repo, all you might need is `build` and `publish-lambda` scripts from `package.json`. Replace `MY_LAMBDA_NAME_HERE` with your AWS lambda function name.

### What's in the box?

`package.json` is extremely minimal: no dependencies, no `start` script, it only creates a production build, compresses it and publishes (replaces) your AWS lambda function.

Here is what `scripts` look like:

```
  "scripts": {
    "compile": "rm -rf dist/* && cp src/index.js dist",
    "build": "yarn compile && yarn install --production --modules-folder dist/node_modules && (cd dist && zip -r lambda.zip .)",
    "publish-lambda": "aws lambda update-function-code --function-name MY_LAMBDA_NAME_HERE --zip-file fileb://dist/lambda.zip",
    "build-publish": "yarn build && yarn publish-lambda"
  }
```

Let's go thru these lines:

`"compile": "rm -rf dist/* && cp src/index.js dist"`
Copies source to dist. If you are using Babel, replace this script with Babel build command (make sure it outputs to `dist` folder).



`"build": "yarn run compile && yarn install --production --modules-folder dist/node_modules && (cd dist && zip -r lambda.zip .)"`
Creates `lambda.zip` file that you can upload thru AWS console or AWS cli. Script compiles files (through `yarn run compile`), installs only production dependencies (thru `yarn install --production`) and compresses everything in dist folder into `lambda.zip` file.



`"publish-lambda": "aws lambda update-function-code --function-name MY_LAMBDA_NAME_HERE --zip-file fileb://dist/lambda.zip"`
Updates AWS lambda function with contents of `lambda.zip`. Replace `MY_LAMBDA_NAME_HERE` with the "real" AWS lambda function name. This script relies on AWS cli tool.
