## Git tag

"Git create tag" to create tag

	git tag <tagname>
	git tag v1.0

Create tag from a different Commit

	git tag v1.0 35a3a6c8e025da8fc6be5058fe6bc4024fd76642

List/View all available Git Tags

	git tag
	git tag -l "v2.*"

Git push tag to remote

	git push origin v1.0
	git push origin --tags


Git delete local tag

	git tag -d v2.5
	
Git delete remote tag

	git push origin :refs/tags/v2.5

Git checkout tag as branch

	git checkout v1.0
	git checkout -b <new-branch-name>

Source: https://www.drupixels.com/blog/git-tags-guide-create-delete-push-tags-remote-and-much-more

## Git branch

Deleting Git Branch from Local

	git branch -d mybranch

Force to delete branch wich is not fully merged

	git branch -D mybranch

Deleting Git Branch from Remote

	git push origin --delete mybranch


## Git merged


	git checkout master
	git merge migracao_repos

