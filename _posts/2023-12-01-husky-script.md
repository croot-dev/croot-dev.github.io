---
layout: post
title: "husky script"
created: 2023-12-01
edited: 2024-03-19
category: [기술]
tags: []
---


## git commit 대상 ts, tsx 파일들의 console을 제거하는 스크립트


```shell
#!/usr/bin/env sh

# Function to remove console.log statements from a file
remove_console() {
  file=$1
  tmp_file="$file.tmp"

  # Use sed to remove console.log statements
  sed '/console\.[debug|log|error|info|trace]/d' "$file" > "$tmp_file"

  # Replace the original file with the temporary file
  mv "$tmp_file" "$file"
}

# Get the list of modified files in the commit range
files=$(git diff --staged --name-only --diff-filter=d)

for file in $files; do
  # Process only TypeScript files
  if [[ "$file" == *.ts || "$file" == *.tsx ]]; then
    echo "Processing $file"
    git show "$commit_range:$file" | remove_console "$file"
    git add "$file"
  fi
done

echo "Console removal complete."
```

