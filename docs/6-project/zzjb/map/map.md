# 🍎 大地图分析

本文分析大地图的代码

## 🌲 ChangePosition_EncounterCheck

这个是走路就会触发的回调, 我们一起看看它在做什么

```cs
using Unity.Mathematics;
/** 
 * @author: - 
 * @createTime: -
 * @updateTime: 2025-7-19
 * @description: 玩家移动时检测是否触发遇敌事件
 * @version: 
 * 1、遇敌逻辑：移动超过一定距离后，按概率判定是否进入战斗
 * 2、增加 PlaceConfig 表的 isMeetEnemy 配置，控制地图是否允许遇敌
 */
namespace ET.Client
{
    /// <summary>
    /// 监听玩家位置变化事件，检测是否触发遇敌
    /// </summary>
    [Event(SceneType.Current)]
    [FriendOf(typeof(UnitInfoComponent))]
    public class ChangePosition_EncounterCheck : AEvent<Scene, ChangePosition>
    {
        private const float checkInterval = 5f; // 移动超过 5 米才判定一次
        private const float encounterProbability = 0.5f; // 50% 概率触发遇敌
        private const int LastCheckPosXKey = 900001; // Numeric 存储上次检测点 X
        private const int LastCheckPosZKey = 900002; // Numeric 存储上次检测点 Z
        private const int HasInitKey = 900003;       // Numeric 是否初始化过（0=未初始化）

        /// <summary>
        /// 事件回调：当玩家位置发生变化时触发
        /// </summary>
        protected override async ETTask Run(Scene scene, ChangePosition args)
        {
            // ① 判断当前地图是否允许遇敌
            PlaceConfig placeConfig = PlaceConfigCategory.Instance.GetByPlace(scene.Name);
            if (placeConfig.IsMeetEnemy == 0)
            {
                // 当前场景配置禁止遇敌，直接返回
                return;
            }

            // ② 获取当前移动的单位（角色）
            Unit unit = args.Unit;
            UnitComponent unitComponent = scene.GetComponent<UnitComponent>();
            if (unitComponent == null || unitComponent.MyUnit == null) return;

            // 只处理本地玩家（主角），其他玩家/NPC 不触发
            if (unit.Id != unitComponent.MyUnit.Id)
                return;

            // ③ 获取或添加 NumericComponent，用于存储上次检测数据
            var numeric = unit.GetComponent<NumericComponent>();
            if (numeric == null)
            {
                numeric = unit.AddComponent<NumericComponent>();
            }

            int hasInit = numeric.GetAsInt(HasInitKey);
            float2 nowPos2D = new float2(unit.Position.x, unit.Position.z);

            // ④ 第一次进入地图：初始化检测点，不触发遇敌
            if (hasInit == 0)
            {
                numeric.Set(LastCheckPosXKey, (int)(nowPos2D.x * 1000));
                numeric.Set(LastCheckPosZKey, (int)(nowPos2D.y * 1000));
                numeric.Set(HasInitKey, 1);
                return;
            }

            // ⑤ 计算与上次检测点的距离
            float lastCheckPosX = numeric.GetAsInt(LastCheckPosXKey) / 1000f;
            float lastCheckPosZ = numeric.GetAsInt(LastCheckPosZKey) / 1000f;
            float distance = math.distance(new float2(lastCheckPosX, lastCheckPosZ), nowPos2D);

            // ⑥ 移动距离达到阈值，进行遇敌判定
            if (distance >= checkInterval)
            {
                // 更新上次检测点
                numeric.Set(LastCheckPosXKey, (int)(nowPos2D.x * 1000));
                numeric.Set(LastCheckPosZKey, (int)(nowPos2D.y * 1000));

                // ⑦ 随机概率判定
                if (RandomGenerator.RandFloat01() < encounterProbability)
                {
                    // 获取角色额外信息（比如玩家编号）
                    var unitInfo = unit.GetComponent<UnitInfoComponent>();                     
                    long beautifulNumber = unitInfo != null ? unitInfo.BeautifulNumber : 0;

                    // 打印战斗开始日志
                    Log.Info($"{unit.Scene().Name} 战斗开始 玩家ID:{unit.Id} 用户ID:{beautifulNumber} 坐标:({unit.Position.x:F2}, {unit.Position.y:F2}, {unit.Position.z:F2})");

                    // ⑧ 向地图服务器发送遇敌请求
                    C2M_MeetEnemy request = C2M_MeetEnemy.Create();
                    request.UnitId = unit.Id;
                    request.Position = unit.Position;

                    // 等待服务器响应
                    M2C_MeetEnemy m2cMeetEnemy = await scene.Root().GetComponent<ClientSenderComponent>().Call(request) as M2C_MeetEnemy;
                    Log.Info($"Error:{m2cMeetEnemy.Error}, Message:{m2cMeetEnemy.Message}");

                    // 如果服务器返回失败，不进入战斗
                    if(m2cMeetEnemy.Error == ErrorCode.ERR_MeetEnemyFail)
                    {
                        // TODO: 这里可以做失败处理逻辑（比如重试提示）
                        return;
                    }

                    // ⑨ 遇敌成功，触发本地事件，进入战斗流程
                    Log.Info($"<color=yellow>-------------------------------------------->启动遇敌!!<------------------------------------------------</color>");
                    EventSystem.Instance.Publish(scene, new MeetEnemyStart() { Scene = scene, Unit = unit });
                }
            }

            // 任务结束
            await ETTask.CompletedTask;
        }
    }
}
```