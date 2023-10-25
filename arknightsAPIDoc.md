# ArkHost V2 API Document

这次更新主要内容是分离了auth服务，新增工单服务。

 auth server | [https://github.com/closure-studio/ReadMe/blob/main/idServerAPIDoc.md]

 ticket server | [https://github.com/closure-studio/ReadMe/blob/main/ticketsAPIDoc.md]


## Game

这次更新整合了一些API，移除了一些旧API，比如geetest和Ocr接口。

请注意，idServer Token 请通过auth server的login api接口获得。

#### Create Game 创建游戏
【Path】

https://api.arknights.host/Game 【post】 

【Header】

    {
    Authorization:{idServer Token}
    X-Platform:{website}
    recaptchaToken:{recaptcha Token}
    }
【Body】

    {"Account": "1892xxxxx" , "Platform": 1, "Password": "xxxxxx"}

【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }
#### Game Login 游戏登陆
如果是官请带上G开头，如果是B，请带上B开头

 在GetGames API会返回该已格式化的账号

【Path】

https://api.arknights.host/Game/Login 【post】 

【Header】

    {
    Authorization:{idServer Token}
    X-Platform:{website}
    recaptchaToken:{recaptcha Token}
    }

【Body】

`{"Account": "G1892xxxxx"}`

【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }
	
#### Get Games 获取所有游戏
请注意，与之前的API不同，geetest的极验信息和游戏设置（保留理智，是否自动作战）整合在一起

【Path】

https://api.arknights.host/Game 【get】 

【Header】

`{Authorization:{idServer Token}}`

【Body】

`nil`

【Response】

	{
	  "code": "int", // 1 表示成功，0表示失败，
	  "data": "[]web.WebGame",
	  "message": "string"
	}

    type WebGame struct{
    Status      *Status             `json:"status"`
    CaptchaInfo *CaptchaInfo        `json:"captcha_info"`
    GameSetting *config.GameSetting `json:"game_config"`
    }

    type Status struct {
        //当前用户状态，-1=登陆失败 0=未开启/未初始化/正在初始化但未登录 1=登录中 2=登陆完成/运行中 3=游戏错误
        Account  string          `json:"account"`
        Platform int             `json:"platform"`
        UUID     string          `json:"uuid"`
        Code     int             `json:"code"`
        Text     string          `json:"text"`
        NickName string          `json:"nick_name"`
        Level    int             `json:"level"`
        Avatar   gamestat.Avatar `json:"avatar"`
        CreatedAt int64           `json:"created_at"` // 该游戏创建的timestamp
        IsVertify bool            `json:"is_vertify"` // 如果未进行短信sms验证，则只有24小时的试用期。


    }
    
    // CaptchaInfo 验证请求信息
    type CaptchaInfo struct {
    	Challenge   string `json:"challenge"`
    	Gt          string `json:"gt"`
    	Created     int64  `json:"created"`
    	CaptchaType string `json:"captcha_type"`
    }
    type GameSetting struct {
    	Account               string   `json:"account"`
    	AccelerateSlot        string   `json:"accelerate_slot"`
    	AccelerateSlot_CN     string   `json:"accelerate_slot_cn"`
    	BattleMaps            []string `json:"battle_maps"`
    	EnableBuildingArrange bool     `json:"enable_building_arrange"`
    	IsAutoBattle          bool     `json:"is_auto_battle"`
    	IsStopped             bool     `json:"is_stopped"`
    	KeepingAP             int      `json:"keeping_ap"`
    	RecruitIgnoreRobot    bool     `json:"recruit_ignore_robot"`
    	RecruitReserve        int      `json:"recruit_reserve"`
    	MapId                 string   `json:"map_id"`
    }
    


#### Get Game 获取单个游戏
游戏设置（保留理智，是否自动作战）和作战截图整合在一起

如果是官请带上G开头，如果是B，请带上B开头

在GetGames API会返回该已格式化的账号

【Path】

https://api.arknights.host/Game/G18XXXX 【post】 

【Header】

`{Authorization:{idServer Token}}`

【Body】

`nil`

【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "PlayerWebDataModel", 
      "message": "string"
    }
    type PlayerWebDataModel struct {
    	Status      *PlayerWebStatus `json:"status"`
    	Consumable  interface{}      `json:"consumable"`
    	Inventory   interface{}      `json:"inventory"` // annonymous map
    	Troop       interface{}      `json:"troop"`
    	Config      interface{}      `json:"config"`
    	Screenshot  interface{}      `json:"screenshot"`
    	LastFreshTs int64            `json:"lastFreshTs"`
    }

#### Delete Game 删除游戏
如果是官请带上G开头，如果是B，请带上B开头

 在GetGames API会返回该已格式化的账号

【Path】

https://api.arknights.host/Game 【Delete】 

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】

`{"Account": "G1892xxxxx"}`

【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }

#### Update Game 更新游戏

这个接口整合了几种操作，geetest验证码，requireOcr，游戏设置

如果Body传入 Account，Config 组合 则会更新Config

对于config，可以传入单个字段或者多个字段。

如果Body传入 Account，CaptchaInfo 组合 则会更新geetest验证码

如果Body传入 Account，RequireOcr 组合 则会要求更新Ocr

如果全部传入，则全部更新



【Path】

https://api.arknights.host/Game 【Delete】 

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】
```

{
	Account     string      `json:"account"` // B1831 G1831 【require】
	Config      *GameConfig  `json:"config"` // 游戏设置 【option】
	CaptchaInfo *CaptchaInfo `json:"captcha_info"` // geetest验证码 【option】
	RequireOcr  *bool  `json:"require_ocr"` //要求Ocr识别 【option】
}


type GameConfig struct {
	IsAutoBattle          *bool    `json:"isAutoBattle"`
	MapId                 *string  `json:"mapId"`
	BattleMaps            *[]string`json:"battleMaps"`
	KeepingAP             *int     `json:"keepingAP"`
	RecruitReserve        *int     `json:"recruitReserve"`
	RecruitIgnoreRobot    *bool    `json:"recruitIgnoreRobot"`
	IsStopped             *bool    `json:"isStopped"`
	EnableBuildingArrange *bool    `json:"enableBuildingArrange"`
	AccelerateSlot_CN     *string  `json:"accelerateSlot_CN"`
}

type CaptchaInfo struct {
	Challenge        string `json:"challenge"`
	GeetestChallenge string `json:"geetest_challenge"`
	GeetestValidate  string `json:"geetest_validate"`
	GeetestSeccode   string `json:"geetest_seccode"`
}


```
【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }
	
#### Get Game Log 获取游戏日志log
如果是官请带上G开头，如果是B，请带上B开头

在GetGames API会返回该已格式化的账号

每一次查询返回offest+500条信息。

建议第一次查询offest=0

建议第二次查询offest=500

建议第三次查询offest=1000

【Path】

https://api.arknights.host/Game/Log/:account/:offest 【Get】 

https://api.arknights.host/Game/Log/G18xxxx/500

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】

``

【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }

    type GameLogEntry struct {
        ID       uint   `gorm:"primaryKey" json:"id"`
        Ts       int64  `gorm:"type:int8;index" json:"ts"`
        Name     string `gorm:"type:varchar(32);index" json:"name"`
        LogLevel uint8  `gorm:"type:smallint" json:"logLevel"`
        Content  string `gorm:"type:text" json:"content"`
    }

    type GameLog struct {
        Logs    []*GameLogEntry `json:"logs"`
        HasMore bool            `json:"hasMore"`
    }


## System

这次更新整合了一些API


#### Get System Config 获取系统设置 【公开】

【Path】

https://api.arknights.host/System/Config 【get】 

【Header】


【Body】



【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }
    type SystemConfig struct {
        IsUnderMaintenance *bool   `json:"isUnderMaintenance"`  是否在进行维护
        IsDebugMode        *bool   `json:"isDebugMode"`         是否在debug模式
        Announcement       *string `json:"announcement"`        公告
        AllowGameLogin     *bool   `json:"allowGameLogin"`      允许游戏登录【未部署】
        AllowGameCreate    *bool   `json:"allowGameCreate"`     允许创建游戏【未部署】
        AllowGameUpdate    *bool   `json:"allowGameUpdate"`     允许更新游戏【未部署】
        AllowGameDelete    *bool   `json:"allowGameDelete"`     允许删除游戏【未部署】
}


#### Get System Ap Cost List 获取名人堂 【公开】

【Path】

https://api.arknights.host/System/ApCostList 【get】 

【Header】


【Body】



【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "[]Info",
      "message": "string"
    }
    type Info struct {
        NickName string          `json:"nickName"`
        Avatar   Avatar `json:"avatar"`
        AdditionalAPCosts int    `json:"additionalAPCosts"`
    }
    type Avatar struct {
        Type string `json:"type"`
        ID   string `json:"id"`
    }

#### Update System Config 更新系统设置

可以传单独的字段

也可以传全部的字段

【Path】

https://api.arknights.host/System/Config 【post】 

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】

    {
        IsUnderMaintenance *bool   `json:"isUnderMaintenance"`  是否在进行维护
        IsDebugMode        *bool   `json:"isDebugMode"`         是否在debug模式
        Announcement       *string `json:"announcement"`        公告
        AllowGameLogin     *bool   `json:"allowGameLogin"`      允许游戏登录【未部署】
        AllowGameCreate    *bool   `json:"allowGameCreate"`     允许创建游戏【未部署】
        AllowGameUpdate    *bool   `json:"allowGameUpdate"`     允许更新游戏【未部署】
        AllowGameDelete    *bool   `json:"allowGameDelete"`     允许删除游戏【未部署】
    }


【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }


#### Update System 更新游戏


【Path】

https://api.arknights.host/System/GameDataUpdate 【post】 

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】


【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "nil",
      "message": "string"
    }


#### Get System status 获取运行情况


【Path】

https://api.arknights.host/System/Status 【get】 

【Header】

    {
    Authorization:{idServer Token}
    }

【Body】


【Response】

    {
      "code": "int", // 1 表示成功，0表示失败，
      "data": "SystemStatus",
      "message": "string"
    }
    type SystemStatus struct {
	    GlobalCurrentAp float64 `json:"globalCurrentAp"` // 全局理智
    }