# 验收清单

- [ ] 在 SW2/SW3/SW4 上执行 `display vlan`，确认 VLAN 10 和 20 已通过 GVRP 动态注册。
- [ ] 在 SW1/SW2/SW3 上执行 `display stp brief`，确认 SW4 的 STP 角色为 `ROOT`。
- [ ] 在 PC1 上执行 `ping 10.1.1.3`，确认同 VLAN 互通。
- [ ] 在 PC1 上执行 `ping 20.1.1.3`，确认跨 VLAN 互通。
- [ ] 在 R1 上执行 `display ip interface brief`，确认子接口 `GE0/0/0.10` 和 `GE0/0/0.20` 状态为 Up。