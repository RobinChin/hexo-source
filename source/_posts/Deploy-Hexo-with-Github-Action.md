---
title: Deploy Hexo with Github Action
date: 2020-05-08 03:00:23
tags: Github Action
---

Using Github Action to deploy Github Page.
1. Add .github/workflows/main.yml in hexo source repo
2. Add deploy key in github.io repo Settings
3. Add secret in hexo source repo Settings

```yaml
name: Deploy

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    name: A job to deploy blog.
    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
        submodules: true
    
    # Caching dependencies to speed up workflows. (GitHub will remove any cache entries that have not been accessed in over 7 days.)
    - name: Cache node modules
      uses: actions/cache@v1
      id: cache
      with:
        path: node_modules
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-
    - name: Install Dependencies
      if: steps.cache.outputs.cache-hit != 'true'
      run: npm ci
    
    # Deploy hexo blog website.
    - name: Deploy
      id: deploy
      uses: sma11black/hexo-action@v1.0.1
      with:
        user_name: robinchin
        user_email: chin_chin0110@hotmail.com
        deploy_key: ${{ secrets.DEPLOY_KEY }} #Deploy_KEY in github.io Settings
    # Use the output from the `deploy` step(use for test action)
    - name: Get the output
      run: |
        echo "${{ steps.deploy.outputs.notify }}"
```
