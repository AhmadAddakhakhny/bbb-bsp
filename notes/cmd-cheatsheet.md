##### Generate a core-image-minimal
> bitbake core-image-minimal

##### heck the dependancies of core-image-minimal
> bitbake -g core-image-minimal
> after executing the above cmd, there would be two generated files 
> "pn-buildlist" : lists all packages that would be bitbaked, but that doesn't mean they will be added on my image.
> and "task-depends.dot"


###### Clean build files of a specific recipe?
> bitbake <recipe-name> -c cleanall
> bitbake nano -c cleanall


###### KAS commands?
```
# How to build a core-image
    $ kas build bbb.yml

kas build bbb.yml -c populate_sdk

kas shell bbb.yml -c "bitbake hmi"
kas shell bbb.yml -c "bitbake core-image-minimal -c populate_sdk_ext"
```