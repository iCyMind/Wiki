# Karabiner

OSX 下的按键精灵

## 定义自己的 hyper 键

```xml
<item>
    <name>Left-Option: Hyperkey(opt_r+shift_r+ctrl_r)</name>
    <identifier>icymind.hyperkey</identifier>
    <autogen>
        __KeyToKey__

        KeyCode::OPTION_L,

        KeyCode::COMMAND_R,
        ModifierFlag::OPTION_R | ModifierFlag::SHIFT_R | ModifierFlag::CONTROL_R
    </autogen>
</item>
```
