### 当一个 rule 既没有命令(Recipes)也没有依赖(Prerequisites)
4.7 Rules without Recipes or Prerequisites
If a rule has no prerequisites or recipe, and the target of the rule is a nonexistent file, then make imagines this target to have been updated whenever its rule is run. This implies that all targets depending on this one will always have their recipe run.

An example will illustrate this:

clean: FORCE
        rm $(objects)
FORCE:

Here the target ‘FORCE’ satisfies the special conditions, so the target clean that depends on it is forced to run its recipe. There is nothing special about the name ‘FORCE’, but that is one name commonly used this way.

As you can see, using ‘FORCE’ this way has the same results as using ‘.PHONY: clean’.

Using ‘.PHONY’ is more explicit and more efficient. However, other versions of make do not support ‘.PHONY’; thus ‘FORCE’ appears in many makefiles. See Phony Targets.

from [GNU/make/Force-Targets](https://www.gnu.org/software/make/manual/html_node/Force-Targets.html)

总结: 当一个 rule 既没有 Prerequisites 且没有 Recipes 时，这个 rule 就被认定为已经被更新，所有依赖这个 rule 的目标的其他目标的命令都需要执行。

### 当一个rule 没有依赖(Prerequisites)

比如：
# 未声明为伪目标的 clean
clean:  # 无依赖，只有命令
    rm -f *.o main

此时，如果 Makefile 所在目录下面有名为 clean 的文件，那么命令 `rm -f *.o main` 不会被执行，
只有当Makefile 所在目录下面没有名为 clean 的文件，才会执行 `rm -f *.o main` 命令。