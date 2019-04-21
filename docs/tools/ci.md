# CI


## circle ci

```yaml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:8.12.0
    working_directory: ~/babalink
    steps:
      - checkout
      - run:
          name: Create hash file
          command: |
            # 每天都生成新的缓存
            date "+%Y%m%d" > /tmp/hash_file
            # production/qa 的缓存分开存储
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              echo 'production' >> /tmp/hash_file
            else
              echo 'qa' >> /tmp/hash_file
            fi
            cat /tmp/hash_file
      - restore_cache: # special step to restore the dependency cache
          key: link-dependencies-v2-{{ checksum "package-lock.json" }}-{{ checksum "/tmp/hash_file" }}
      - run:
          name: Install Dependencies
          command: |
            # master 分支在还原缓存后始终做一步安装的操作(不完全相信缓存)
            # 其他分支仅在找不到缓存时才做安装依赖的操作(完全相应缓存)
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              npm install --no-save
            elif [ ! -d node_modules ]; then
              npm install --no-save
            fi
      - run:
          name: Building
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
                npm run build
            else
                npm run build:qa
            fi
      - save_cache:
          name: Save NPM Package Cache
          key: link-dependencies-v2-{{ checksum "package-lock.json" }}-{{ checksum "/tmp/hash_file" }}
          paths:
            - node_modules
      - persist_to_workspace:
          root: .
          paths:
            - dist

  deploy:
    docker:
      - image: circleci/node:8.12.0
    working_directory: ~/babalink
    steps:
      - run:
          name: Install rsync
          command: sudo apt install -y rsync
      - attach_workspace:
          at: .
      - deploy:
          name: Deploy to Server
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
                DEPLOY_HOST="ir.88meeting.com"
            else
                DEPLOY_HOST="ir.88meeting-qa.com"
            fi
            rsync -e "ssh -o StrictHostKeyChecking=no" -azv  --exclude="*.html" dist/ 88team@$DEPLOY_HOST:/var/www/ir-site/app/dist
            rsync -e "ssh -o StrictHostKeyChecking=no" -azvc --include="*.html" --exclude="*.*"   dist/ 88team@$DEPLOY_HOST:/var/www/ir-site/app/dist

workflows:
  version: 2
  build-deploy-qa:
    jobs:
      - build:
          filters:
            branches:
              ignore: master
      - hold-before-deploy-qa:
          type: approval
          requires:
            - build
      - deploy:
          requires:
            - hold-before-deploy-qa
  build-deploy-production:
    jobs:
      - build:
          filters:
            branches:
              only: master
      - deploy:
          requires:
            - build
  nightly-build: # 每天(UTC: 00:10) 构建一次 master, 以构建缓存
    triggers:
      - schedule:
          cron: "10 00 * * *"
          filters:
            branches:
              only:
                - master
    jobs:
      - build

```

## async
通过 checksum 而不是修改时间和文件大小来决定一个文件该不该被传输及覆盖. 参数: `-c`:

>-c, --checksum              skip based on checksum, not mod-time & size

