
# [zellij: user guide](https://zellij.dev/documentation/)
  - ## [Zellij Layouts](https://zellij.dev/documentation/layouts.html)
```
#vi ${HOME}/zellij-layouts/split-pane-1.kdl

#zellij --layout ${HOME}/zellij-layouts/split-pane-2.kdl

zellij --layout ${HOME}/zellij-layouts/split-pane-3.kdl

--

#########
# layouts
#
zellij setup --dump-layout default > ~/zellij-layouts/my-quickstart-layout-file.kdl

zellij --layout ${HOME}/zellij-layouts/split-pane-1.kdl

```

