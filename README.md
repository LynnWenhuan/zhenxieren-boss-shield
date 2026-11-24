# 《镇邪人》复刻版 — BOSS 战破盾与 30 秒狂暴倒计时项目初始化

本项目为《镇邪人》复刻版 BOSS 战核心战斗机制（破盾机制 + 30 秒狂暴倒计时）的 Unity 工程初始化模板，包含核心 C# 状态机脚本、ScriptableObject 参数配置类、JSON 数值配置表以及初始化研发任务看板。

## 核心数值公式
- 护盾上限：S_max = BaseShield * (1 + 0.15 * (L_stage - 1))（建议设为小队 18~22 秒完整循环总削韧值，留出 8~12 秒容错）
- 持盾 HP 减伤：D_hp_shielded = D_raw * (1 - 0.75)
- 削韧伤害：D_poise = P_skill * (1 + B_break) * M_element * M_gesture * M_charging
- 破盾虚弱易伤（4.5s）：D_hp_stunned = D_raw * (1 + 0.80 + B_stun_dmg)
- 30秒超时大招惩罚：D_ultimate = PlayerMaxHP * 0.85 * (1 + 0.25 * N_cycle)
- 二次重铸护盾膨胀：S_max(N_cycle) = S_max(0) * (1 + 0.30 * N_cycle)

## 四态状态机流转
- Phase_Shielded（30.0s）：护盾蓄力期，红圈蓄力期间破盾易伤 +30%
- Phase_BrokenStun（4.5s）：破盾虚弱期，触发 0.08s 顿帧（timeScale = 0.05f）与 180% 爆发易伤
- Phase_UltimateCast（2.0s）：大招惩罚期，第 1.5s 结算全屏真伤
- Phase_ShieldRecover（1.2s）：护盾重铸期，cycleCount++，按 30% 膨胀补满护盾并击退近战
