# git-hooks-test
Git Hooks를 사용하여 pre-commit hook을 작성해보자.

## prepare-commit-msg
```shell
COMMIT_EDITMSG=$1

addBranchNumber() {
  NAME=$(git branch | grep '*' | sed 's/* //' | cut -d '-' -f1)
  ISSUE_TYPE=$(echo $NAME | cut -d '/' -f1)
  ISSUE_NUMBER=$(echo $NAME | cut -d '/' -f2)
  DESCRIPTION=$(git config branch."$NAME".description)
  echo "$ISSUE_TYPE: $(cat $COMMIT_EDITMSG) (#$ISSUE_NUMBER)" > $COMMIT_EDITMSG
  if [ -n "$DESCRIPTION" ]
  then
     echo "" >> $COMMIT_EDITMSG
     echo $DESCRIPTION >> $COMMIT_EDITMSG
  fi
}

MERGE=$(cat $COMMIT_EDITMSG|grep -i 'merge'|wc -l)

if [ $MERGE -eq 0 ] ; then
  addBranchNumber
fi
```