---
layout: post
title: "Notion Database 와 Github Pages 연동하기"
created: 2023-12-02
edited: 2024-01-03
category: [기술]
tags: [Notion:rgb(250 222 201):rgb(73 41 14),Github Pages:rgba(227 226 224 0.5):rgb(50 48 44)]
---


## Intro


요즘은 다 fork 떠서 만들긴 하던데 간만에 삽질도 좀 해볼 겸 직접 해보았다.


## 준비

1. Notion 템플릿 작성
	1. 
2. API 토큰 생성
	1. Notion **`프라이빗 API 통합 시크릿`** 생성하기
	🔗 [https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)
	2. Notion database ID

		🔗 [https://developers.notion.com/reference/retrieve-a-database](https://developers.notion.com/reference/retrieve-a-database)

	3. Github Token
	🔗 [https://github.com/settings/tokens](https://github.com/settings/tokens)

		**`Generate New Token(Classic)`** 선택 후 **`repo`**, **`workflow`**, **`admin:repo_hook`** 세가지를 체크하여 토큰 생성.


	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">나는 로컬에서 테스트하기 위해 <code class="language-plaintext highlighter-rouge">.env.local</code> 에다가 저장했다.</div>
	</div>


	```yaml
	# .env.local
	NOTION_TOKEN=*****
	DATABASE_ID=*****
	COMMIT_TOKEN=*****
	```


## 환경 설치


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">아래 설치에서는 <a href="https://brew.sh/" target="_blank">homebrew</a>를 이용한다.</div>
</div>

1. [**`rbenv`**](https://github.com/rbenv/rbenv), [**`nvm`**](https://github.com/nvm-sh/nvm) 설치

	```shell
	brew install rbenv nvm
	```


	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><code class="language-plaintext highlighter-rouge"><strong>rbenv</strong></code> : Ruby 버전을 관리하기 위한 패키지<br /><code class="language-plaintext highlighter-rouge"><strong>nvm</strong></code> : Node 버전을 관리하기 위한 패키지</div>
	</div>

2. `Node 18` 설치

	```shell
	nvm install 18
	nvm use 18
	```

3. `Ruby 3.1.0` 설치

	```yaml
	# ruby 3.1.0 설치
	rbenv install 3.1.0
	rbenv rehash
	rbenv global 3.1.0
	```


	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;"><code class="language-plaintext highlighter-rouge"><strong>rbenv</strong></code>는 여러 루비 버전을 관리하기 위한 패키지이다.</div>
	</div>

4. **`bundler`**(루비 패키지 관리 프로그램) 설치

	```shell
	# Permission 문제로 sudo 이용하여 실행.
	sudo gem install bundler
	```


## 설정파일


<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">파일 경로는 프로젝트 루트 경로를 최상단으로 정의하여 작성함.</div>
</div>

1. **`package.json`** 생성

	```shell
	# yarn 사용 시
	yarn init
	# npm 사용 시
	npm init
	```


	> 프로젝트 상세 내용은 알아서 작성

2. **`jekyll`** 설치

	```shell
	bundler add jekyll
	bundler exec jekyll new . --skip-bundle --force
	```

3. 설정파일 생성
	- **`/package.json`** : 프로젝트 정보 파일

		```javascript
		{
		  "name": "croot-dev.github.io",
		  "version": "1.0.0",
		  "main": "index.js",
		  "repository": "https://github.com/croot-dev/croot-dev.github.io.git",
		  "author": "croot <croot.dev@gmail.com>",
		  "private": true,
		  "scripts": {
		    "generate": "cross-env NODE_ENV=local ENV_PATH=.env.local node ./_scripts/notionPageImporter",
		    "build": "bundle exec jekyll build",
		    "serve": "bundle exec jekyll serve"
		  },
		  "devDependencies": {
		    "@notionhq/client": "^1.0.4",
		    "@types/node-fetch": "^2.6.2",
		    "cross-env": "^7.0.3",
		    "dayjs": "^1.11.10",
		    "dotenv": "^16.3.1",
		    "notion-to-md": "^2.5.5"
		  },
		  "dependencies": {
		    "axios": "^1.6.2"
		  }
		}
		```

	- **`/Gemfile`** : Ruby 의존성 정의 파일

		```shell
		source "https://rubygems.org"
		
		gem "jekyll", "~> 3.9.3"
		# gem "minima", "~> 2.5"
		gem "github-pages", "~> 228", group: :jekyll_plugins
		group :jekyll_plugins do
		  gem "jekyll-paginate-v2"
		  gem "jekyll-sitemap"
		end
		
		platforms :mingw, :x64_mingw, :mswin, :jruby do
		  gem "tzinfo", ">= 1", "< 3"
		  gem "tzinfo-data"
		end
		
		gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
		
		gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
		gem "webrick", "~> 1.8"
		gem "html-proofer", "~> 5.0"
		```

	- **`/_scripts/notionPageImporter.ts`** : 노션 페이지를 마크다운으로 변환하는 스크립트

		```javascript
		// notionPageImporter.ts
		const { Client } = require("@notionhq/client");
		const { NotionToMarkdown } = require("notion-to-md");
		const dayjs = require("dayjs");
		const path = require("path");
		const fs = require("fs");
		const axios = require("axios");
		
		// 노션 데이터베이스 속성명
		const PROPERTY = {
		  PUBLISH: '공개', // 타입: 체크박스(checkbox)
		  TITLE: '게시물', // 타입: 제목(plain_text)
		  CATEGORY: '카테고리',
		  TAGS: '태그', // 타입: 다중선택(multi_select)
		  
		}
		const DEFAULT_CATEGORY_NAME = '기타'; // 카테고리 없을 시 기본으로 적용할 카테고리 명
		
		// Using dotenv in local
		if (process.env.NODE_ENV === 'local') {
		  require('dotenv').config({ path: process.env.ENV_PATH, override: true });
		}
		
		const notion = new Client({
		  auth: process.env.NOTION_TOKEN,
		});
		
		const rootDir = "_posts";
		fs.mkdirSync(rootDir, { recursive: true });
		
		(async () => {
		
		  const databaseId = process.env.DATABASE_ID;
		  // TODO has_more
		  const response = await notion.databases.query({
		    database_id: databaseId,
		    filter: {
		      property: PROPERTY.PUBLISH,
		      checkbox: {
		        equals: true,
		      },
		    },
		  });
		  for (const { id, properties, created_time, last_edited_time } of response.results) {
		    // date
		    const createdDate = dayjs(created_time).format("YYYY-MM-DD");
		    const updatedDate = dayjs(last_edited_time).format("YYYY-MM-DD");
		    
		    // title
		    const tempTitle = properties?.[PROPERTY.TITLE]?.["title"];
		    const title = tempTitle.length > 0? tempTitle[0]?.["plain_text"] : id;
		
		    // category
		    const category = properties?.[PROPERTY.CATEGORY]?.["select"]?.name || DEFAULT_CATEGORY_NAME;
		
		    // tags
		    const tagList = properties?.[PROPERTY.TAGS]?.["multi_select"] || [];
		    const tags = tagList.map((tag) => `${tag['name']}`);
		
		    // frontmatter
		    const frontmatter = `---
		layout: post
		title: "${title}"
		created: ${createdDate}
		edited: ${updatedDate}
		category: [${category}]
		tags: [${tags.join(',')}]
		---
		
		`;
		    
		    // passing notion client to the option
		    const n2m = new NotionToMarkdown({ notionClient: notion });
		    const blocks = await n2m.pageToMarkdown(id);
		    const markdown = n2m.toMarkdownString(blocks)["parent"];
		    const fileTitle = `${createdDate}-${title.replaceAll(" ", "-")}.md`;
		
		    if(!markdown) { continue; }
		
		    let imageIndex = 0;
		    const edited_markdown = markdown.replace(
		      /!\[(.*?)\]\((.*?)\)/g,
		      (match, p1, p2, p3) => {
		        const dirname = path.join("assets/img", fileTitle);
		        if (!fs.existsSync(dirname)) {
		          fs.mkdirSync(dirname, { recursive: true });
		        }
		        const filename = path.join(dirname, `${imageIndex}.png`);
		
		        axios.get(p2, { responseType: "stream" })
		          .then(function (response) {
		            let file = fs.createWriteStream(`${filename}`);
		            response.data.pipe(file);
		          })
		          .catch(function (error) {
		            console.log(error);
		          });
		
		        const res = (p1 === '')? '' : `_${p1}_`;
		        return `![${imageIndex++}]` + `(/${filename})` + `${res}`;
		      });
		
		    // Write file
		    fs.writeFile(path.join(rootDir, fileTitle), frontmatter + edited_markdown, (err) => {
		      if (err) {
		        console.log(err);
		      }
		    });
		  }
		})();
		```

	- **`/.github/workflows/pages-deploy.yaml`** : github action 스크립트

		```yaml
		name: "Build and Deploy"
		on:
		  repository_dispatch:
		    types: [RUN_WORKFLOW_DISPATCH]
		      
		permissions:
		  contents: write
		  pages: write
		  id-token: write
		
		# Allow one concurrent deployment
		concurrency:
		  group: "pages"
		  cancel-in-progress: true
		
		jobs:
		  importer:
		    runs-on: ubuntu-latest
		
		    steps:
		      - uses: actions/checkout@master
		    
		      - name: Clean Directory
		        run: |
		          for file in assets/img/*
		          do
		              if [[ $file != "assets/img/favicons" ]]
		              then
		                  rm -rf "$file"
		              fi
		          done
		          rm -rf _site
		      
		      - uses: actions/setup-node@v2
		        with:
		          node-version: "17"
		
		      - run: npm install
		
		      - run: node _scripts/notionPageImporter.js
		        env:
		          NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
		          DATABASE_ID: ${{ secrets.DATABASE_ID }}
		
		      - uses: stefanzweifel/git-auto-commit-action@v4
		        env:
		          GITHUB_TOKEN: ${{ secrets.COMMIT_TOKEN }}
		        with:
		          commit_message: "[배포] Notion 변경 사항 저장"
		          branch: main
		          commit_user_name: importer-bot 🤖
		          commit_user_email: actions@github.com
		          commit_author: importer-bot 🤖 <actions@github.com>
		 
		  build:
		    needs: importer
		    runs-on: ubuntu-latest
		
		    steps:           
		      - name: Checkout
		        uses: actions/checkout@v3
		        with:
		          ref: main
		          fetch-depth: 1
		          # submodules: true
		          # If using the 'assets' git submodule from Chirpy Starter, uncomment above
		          # (See: https://github.com/cotes2020/chirpy-starter/tree/main/assets)
		
		      - name: Setup Pages
		        id: pages
		        uses: actions/configure-pages@v1
		
		      - name: Setup Ruby
		        uses: ruby/setup-ruby@v1
		        with:
		          ruby-version: '3.1' # reads from a '.ruby-version' or '.tools-version' file if 'ruby-version' is omitted
		          bundler-cache: true
		
		      - name: Build site
		        run: bundle exec jekyll b -d "_site"
		        env:
		          JEKYLL_ENV: "production"
		
		      - name: Test site
		        run: |
		          bundle exec htmlproofer _site --disable-external --checks --allow_hash_href
		
		      - name: Upload site artifact
		        uses: actions/upload-pages-artifact@v1
		        with:
		          path: "_site"
		
		  deploy:
		    environment:
		      name: github-pages
		      url: $
		    runs-on: ubuntu-latest
		    needs: build
		    steps:
		      - name: Deploy to GitHub Pages
		        id: deployment
		        uses: actions/deploy-pages@v1
		```

	- **`.env.local`**  (준비 > API 토큰 생성 단계 참고하여 작성)

		```yaml
		NOTION_TOKEN=#YOUR_NOTION_TOKEN
		DATABASE_ID=#YOUR_DATABASE_ID
		COMMIT_TOKEN=#YOUR_COMMIT_TOKEN
		```


## 테스트


```shell
yarn test
yarn serve
```


> [http://localhost:4000](http://localhost:4000/) 접속하여 확인 가능.


## 배포

1. Repository secrets 추가

	https://github.com/**[Github ID]**/**[Repository Name]**/settings/secrets/actions


	위 주소로 접근하여 아래와 같이 값들을 넣어준다. (위에 생성한 **`.env.local`** 참고)


	![0](/assets/img/2023-12-02-Notion-Database-와-Github-Pages-연동하기.md/0.png)_github secrets actions_

2. 배포 요청 API 호출하는 HTML 생성 (**`COMMIT_TOKEN`** 수정 입력 필요)

	```html
	<!DOCTYPE html>
	<html>
	<head>
	  <meta charset="UTF-8">
	</head>
	<body>
	  <button id="triggerButton" class="trigger-button">갱신</button>
	  <div id="message" class="message"></div>
	
	  <script>
	  document.getElementById("triggerButton").addEventListener("click", function() {
	    var messageElement = document.getElementById("message");
	    messageElement.textContent = "요청 전송 중...";
	
	    var xhr = new XMLHttpRequest();
	    xhr.open("POST", "https://api.github.com/repos/croot-dev/croot-dev.github.io/dispatches", true);
	    xhr.setRequestHeader("Accept", "application/vnd.github.v3+json");
	    xhr.setRequestHeader("Authorization", "Bearer [YOUR_COMMIT_TOKEN]");
	    xhr.setRequestHeader("Content-Type", "application/json");
	
	    xhr.onload = function() {
	      if (xhr.status === 204) {
	        messageElement.textContent = "요청이 성공적으로 전송되었습니다." + xhr.status;
	      } else {
	        messageElement.textContent = "요청 전송에 실패했습니다.<br>상태 코드: " + xhr.status;
	      }
	    };
	
	    xhr.onerror = function() {
	      messageElement.textContent = "요청 전송 중 알 수 없는 오류가 발생했습니다.";
	    };
	
	    xhr.send(JSON.stringify({"event_type": "RUN_WORKFLOW_DISPATCH"}));
	  });
	</script>
	</body>
	</html>
	```


	<div class="callout" style="display:flex;width:100%;border-radius:4px;background:rgb(241,241,239);padding: 16px 16px 16px 12px;">
	<div style="display:flex;align-items:center;justify-content:center;height:24px;width:24px;border-radius:0.25em;flex-shrink:0;">💡</div>
	<div style="white-space:pre-wrap;word-break:break-word;caret-color:rgb(55, 53, 47);margin-left:8px;padding-left:2px;padding-right:2px;">로컬에서 HTML 파일을 열어서 버튼을 누르면 github action이 실행된다.</div>
	</div>


## 확인


http://[Github ID].github.io 주소로 접근하여 배포 확인.


## **Troubleshooting**


 **Cause:**  로컬과 Github Actions 플랫폼 불일치로 인한 오류


```shell
> bundle install
/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle config --local path /home/runner/work/croot-dev.github.io/croot-dev.github.io/vendor/bundle
/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle config --local deployment true
Cache key: setup-ruby-bundler-cache-v6-ubuntu-22.04-x64-ruby-3.1.4-wd-/home/runner/work/croot-dev.github.io/croot-dev.github.io-with--without--only--Gemfile.lock-073fe0d893d3d26402573dc722ed2240f3b54e2043dff09f2f61ef058a7aecd1
/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle install --jobs 4
Your bundle only supports platforms ["x86_64-darwin-22"] but your local platform
is x86_64-linux. Add the current platform to the lockfile with
`bundle lock --add-platform x86_64-linux` and try again.
Error: The process '/opt/hostedtoolcache/Ruby/3.1.4/x64/bin/bundle' failed with exit code 16
```


 **Resolve:**   Github Actions 맞추어 lockfile 수정.


```shell
bundle lock --add-platform x86_64-linux
```


## References


[https://aymanbagabas.com/blog/2022/03/29/import-notion-pages-to-jekyll.html](https://aymanbagabas.com/blog/2022/03/29/import-notion-pages-to-jekyll.html)


[https://lourcode.kr/posts/Jekyll-기반-Github-Pages와-Notion-Page-연동/](https://lourcode.kr/posts/Jekyll-%EA%B8%B0%EB%B0%98-Github-Pages%EC%99%80-Notion-Page-%EC%97%B0%EB%8F%99/)

