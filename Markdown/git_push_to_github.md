# How to push code/file to github by git

## 1. create local respository
initialize the local respository
```
git init
```


## 2. switch your branch
make sure your branch is consistent with github
```
git branch -M main
```

## 3. link to github
```
git remote add github .......
```
check the remote repository
```
git remote -v
```

## 4. pull from repository
if you change the remote repository,you should pull before you push or add... 
``` 
git pull github "main"
```

## 5. check workspace
```
git status
```

## 6. add you changes
```
git add .
```

## 7.commit your changes
```
git commit -m "..."
```

## 8.push to the remote repository
```
git push github "main"
```