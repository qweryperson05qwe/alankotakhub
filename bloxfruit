-- ============================================================
--   ALANKOTAK HUB v5.1 — MOBILE FINAL
--   Owner: UserId 7640741142
--   Keys: alankaka (24h) | amirkaka (7d)
--   Всё работает на телефоне. RightShift убран.
-- ============================================================

getgenv().AKH_Team      = getgenv().AKH_Team or "Pirates"
getgenv().AKH_FixCrash  = getgenv().AKH_FixCrash or false
getgenv().AKH_FixCrash2 = getgenv().AKH_FixCrash2 or false

if _G._akh_debounce and (tick() - _G._akh_debounce) < 3 then
    return warn("[AKH] Уже запущен")
end
_G._akh_debounce = tick()

-- ===== СЕРВИСЫ =====
local Players      = game:GetService("Players")
local RunService   = game:GetService("RunService")
local VirtualUser  = game:GetService("VirtualUser")
local StarterGui   = game:GetService("StarterGui")
local UserInput    = game:GetService("UserInputService")
local Lighting     = game:GetService("Lighting")
local TweenService = game:GetService("TweenService")
local CoreGui      = game:GetService("CoreGui")
local HttpService  = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")

local LocalPlayer = Players.LocalPlayer
local Camera      = workspace.CurrentCamera

local VIM
pcall(function() VIM = game:GetService("VirtualInputManager") end)

-- ===== OWNER =====
local OWNER_USERID = 7640741142
local ADMIN_USERIDS = { [7640741142] = true }
local function IsOwner() return LocalPlayer and LocalPlayer.UserId == OWNER_USERID end
local function IsAdminUser() return LocalPlayer and ADMIN_USERIDS[LocalPlayer.UserId] == true end

-- ===== KEY SYSTEM =====
local VALID_KEYS = {
    ["alankaka"] = { tier="normal",  display="Normal",    duration=24*3600 },
    ["amirkaka"] = { tier="premium", display="PREMIUM",   duration=7*24*3600 },
}

local KEY_FILE    = "AKH_key.txt"
local RANK_FILE   = "AKH_rank.txt"
local EXPIRE_FILE = "AKH_expire.txt"

getgenv().AKH_Tier = getgenv().AKH_Tier or "none"
local TIER_LEVELS = { none=0, normal=1, premium=2, administrateur=3 }

local function CheckKey(k) return VALID_KEYS[k] ~= nil end
local function GetTier(k) local d=VALID_KEYS[k]; return d and d.tier end
local function GetDuration(k) local d=VALID_KEYS[k]; return d and d.duration or 0 end
local function GetTierLevel(t) return TIER_LEVELS[t or getgenv().AKH_Tier] or 0 end

local function IsNormal()  return GetTierLevel() >= 1 end
local function IsPremium() return GetTierLevel() >= 2 end
local function IsAdmin()   return GetTierLevel() >= 3 end

local function FormatTime(sec)
    sec = math.floor(sec)
    local d = math.floor(sec/86400)
    local h = math.floor((sec%86400)/3600)
    local m = math.floor((sec%3600)/60)
    local s = sec % 60
    if d > 0 then return string.format("%dd %dh", d,h) end
    if h > 0 then return string.format("%dh %dm", h,m) end
    if m > 0 then return string.format("%dm %ds", m,s) end
    return string.format("%ds", s)
end

local function SaveKey(k)
    if not writefile then return end
    pcall(writefile, KEY_FILE, k)
    local t = GetTier(k); if t then pcall(writefile, RANK_FILE, t) end
    local exists = false
    if isfile then local ok,e = pcall(isfile, EXPIRE_FILE); if ok and e then exists = true end end
    if not exists then pcall(writefile, EXPIRE_FILE, tostring(os.time() + GetDuration(k))) end
end

local function LoadFile(n)
    if readfile and isfile then
        local ok,e = pcall(isfile, n)
        if ok and e then
            local ok2,c = pcall(readfile, n)
            if ok2 then return c end
        end
    end
end

local function LoadKey() return LoadFile(KEY_FILE) end
local function LoadRank() return LoadFile(RANK_FILE) end
local function LoadExpire() local v = LoadFile(EXPIRE_FILE); return v and tonumber(v) end
local function SaveExpire(t) if writefile then pcall(writefile, EXPIRE_FILE, tostring(t)) end end
local function GetRemainingTime() local e = LoadExpire(); if not e then return 0 end; return math.max(0, e-os.time()) end
local function IsExpired() local e = LoadExpire(); if not e then return false end; return os.time() >= e end
local function ClearAll()
    if delfile then pcall(delfile, KEY_FILE); pcall(delfile, RANK_FILE); pcall(delfile, EXPIRE_FILE) end
end

-- ===== KEY GUI =====
local function ShowKeyGUI(onSuccess)
    local old = CoreGui:FindFirstChild("AKH_KeyGUI"); if old then old:Destroy() end

    local SG = Instance.new("ScreenGui")
    SG.Name = "AKH_KeyGUI"; SG.ResetOnSpawn = false; SG.Parent = CoreGui

    local Main = Instance.new("Frame")
    Main.Size = UDim2.new(0, 320, 0, 400)
    Main.Position = UDim2.new(0.5, -160, 0.5, -200)
    Main.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    Main.BorderSizePixel = 0; Main.Parent = SG
    Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 12)
    local strk = Instance.new("UIStroke", Main)
    strk.Color = Color3.fromRGB(100, 50, 200); strk.Thickness = 2

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, 0, 0, 60); Title.BackgroundTransparency = 1
    Title.Text = "Alankotak Hub"; Title.TextColor3 = Color3.fromRGB(240,240,245)
    Title.Font = Enum.Font.GothamBold; Title.TextSize = 26; Title.Parent = Main

    local Info = Instance.new("TextLabel")
    Info.Size = UDim2.new(1, -40, 0, 80); Info.Position = UDim2.new(0, 20, 0, 70)
    Info.BackgroundColor3 = Color3.fromRGB(28,28,38); Info.BorderSizePixel = 0
    Info.Text = "alankaka — 24 часа\namirkaka — 7 дней"
    Info.TextColor3 = Color3.fromRGB(160,160,180); Info.Font = Enum.Font.Gotham
    Info.TextSize = 15; Info.TextXAlignment = Enum.TextXAlignment.Center
    Info.Parent = Main
    Instance.new("UICorner", Info).CornerRadius = UDim.new(0, 8)

    local Input = Instance.new("TextBox")
    Input.Size = UDim2.new(1, -40, 0, 55); Input.Position = UDim2.new(0, 20, 0, 165)
    Input.BackgroundColor3 = Color3.fromRGB(30,30,40); Input.BorderSizePixel = 0
    Input.Text = ""; Input.PlaceholderText = "Введите ключ"
    Input.PlaceholderColor3 = Color3.fromRGB(100,100,115)
    Input.TextColor3 = Color3.fromRGB(240,240,245); Input.Font = Enum.Font.Gotham
    Input.TextSize = 18; Input.ClearTextOnFocus = false; Input.Parent = Main
    Instance.new("UICorner", Input).CornerRadius = UDim.new(0, 8)

    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(1, -40, 0, 55); Btn.Position = UDim2.new(0, 20, 0, 235)
    Btn.BackgroundColor3 = Color3.fromRGB(100,50,200); Btn.Text = "ПРОВЕРИТЬ"
    Btn.TextColor3 = Color3.fromRGB(240,240,245); Btn.Font = Enum.Font.GothamBold
    Btn.TextSize = 18; Btn.BorderSizePixel = 0; Btn.AutoButtonColor = false
    Btn.Parent = Main
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 8)

    local Status = Instance.new("TextLabel")
    Status.Size = UDim2.new(1, -40, 0, 30); Status.Position = UDim2.new(0, 20, 0, 300)
    Status.BackgroundTransparency = 1; Status.Text = ""
    Status.TextColor3 = Color3.fromRGB(255,80,80); Status.Font = Enum.Font.GothamBold
    Status.TextSize = 15; Status.Parent = Main

    local TimeLabel = Instance.new("TextLabel")
    TimeLabel.Size = UDim2.new(1, -40, 0, 25); TimeLabel.Position = UDim2.new(0, 20, 0, 335)
    TimeLabel.BackgroundTransparency = 1; TimeLabel.Text = ""
    TimeLabel.TextColor3 = Color3.fromRGB(130,220,130); TimeLabel.Font = Enum.Font.Gotham
    TimeLabel.TextSize = 14; TimeLabel.Parent = Main

    local function TryKey()
        local k = Input.Text
        if k == "" then Status.Text = "Введите ключ"; Status.TextColor3 = Color3.fromRGB(255,200,80); return end
        if CheckKey(k) then
            local tier = GetTier(k); local dur = GetDuration(k)
            getgenv().AKH_Tier = tier
            SaveExpire(os.time() + dur)
            if tier == "premium" then
                Status.Text = "ПРЕМИУМ ПРИНЯТ"
                Status.TextColor3 = Color3.fromRGB(255,215,0)
            else
                Status.Text = "КЛЮЧ ПРИНЯТ"
                Status.TextColor3 = Color3.fromRGB(80,255,120)
            end
            TimeLabel.Text = FormatTime(dur)
            SaveKey(k); task.wait(1.2); SG:Destroy()
            if onSuccess then onSuccess() end
        else
            Status.Text = "НЕВЕРНЫЙ КЛЮЧ"; Status.TextColor3 = Color3.fromRGB(255,80,80)
            Input.Text = ""
        end
    end

    Btn.MouseButton1Click:Connect(TryKey)
    Input.FocusLost:Connect(function(e) if e then TryKey() end end)
end

-- ============================================================
--                    MAIN HUB
-- ============================================================
local function StartHub()

_G.AKH = {
    -- Farm
    AutoFarm=true, AutoQuest=true, BringMobs=true, AirFarm=true, KillAura=true,
    AutoStats=false, AutoSkill=false, AutoRaid=false,
    SkillOrder={"Z","X","C","V"}, SkillDelay=0.3,
    AutoFruitSniper=false, FruitSniperRare=true,

    -- Movement
    SpeedHack = not getgenv().AKH_FixCrash2, SpeedValue=60,
    Fly=false, FlySpeed=80, InfiniteJump=false, Noclip=false,
    ClickTP=false, WaterWalk=false, Dash=false,

    -- Combat
    SilentAim=true, HitboxExpand=false, HitboxSize=15,
    BossFarm=false, BossName=nil,
    AutoKen=false, AutoBuso=false,

    -- Visual
    ESP=false, ESPNames=true, ESPHealth=true, ESPDistance=true,
    ESPFruits=false, ESPFruitsRare=true, ESPBerries=false,
    FullBright=false, NoFog=false, RemoveGrass=false,

    -- Misc
    AntiAFK=true, AutoReconnect=true, Team=getgenv().AKH_Team,

    -- Tunables
    AirHeight=50, MobSafeDistance=12, MobClusterRadius=3,
    AuraRange=20, AuraDelay=0.05,
    LoopDelay = getgenv().AKH_FixCrash and 0.2 or 0.05,
    QuestCheckDelay=3, QuestMobName=nil,
}

local function notify(t, txt, d)
    pcall(function()
        StarterGui:SetCore("SendNotification", {Title="Alankotak | "..t, Text=txt, Duration=d or 3})
    end)
end

local function GetChar()
    local c = LocalPlayer.Character
    if not c then return nil,nil,nil end
    return c, c:FindFirstChild("HumanoidRootPart"), c:FindFirstChild("Humanoid")
end

local function GetLevel()
    local ok, lvl = pcall(function() return LocalPlayer.Data.Level.Value end)
    return ok and lvl or 1
end

local function GetGroundY(pos)
    local rp = RaycastParams.new()
    rp.FilterType = Enum.RaycastFilterType.Exclude
    rp.FilterDescendantsInstances = {LocalPlayer.Character}
    local r = workspace:Raycast(pos, Vector3.new(0,-500,0), rp)
    return r and r.Position.Y or 0
end

local function SafeLoop(n, fn, d)
    task.spawn(function()
        while _G.AKH.AutoFarm do
            local ok, err = pcall(fn)
            if not ok then warn("[AKH/"..n.."] "..tostring(err)); task.wait(2) end
            task.wait(d)
        end
    end)
end

local function SafeInput(kc)
    if not VIM then return end
    pcall(function()
        VIM:SendKeyEvent(true, kc, false, game)
        task.wait(0.05)
        VIM:SendKeyEvent(false, kc, false, game)
    end)
end

-- ===== FARM =====
local function HoverCharacter()
    local _, hrp, hum = GetChar()
    if not hrp or not hum then return end
    hrp.Velocity = Vector3.zero; hrp.RotVelocity = Vector3.zero
    pcall(function() hum:ChangeState(Enum.HumanoidStateType.Physics) end)
    local gy = GetGroundY(hrp.Position)
    local t = Vector3.new(hrp.Position.X, gy + _G.AKH.AirHeight, hrp.Position.Z)
    hrp.CFrame = CFrame.new(t) * (hrp.CFrame - hrp.CFrame.Position)
end

-- ===== QUEST DATA (расширенная) =====
local QuestData = {
    -- First Sea
    [1]  = {NPC="Quest Giver", QuestId=1, isBoss=false, mob="Bandit"},
    [10] = {NPC="Quest Giver", QuestId=2, isBoss=false, mob="Monkey"},
    [20] = {NPC="Quest Giver", QuestId=3, isBoss=false, mob="Pirate"},
    [30] = {NPC="Quest Giver", QuestId=4, isBoss=false, mob="Brute"},
    [40] = {NPC="Quest Giver", QuestId=5, isBoss=false, mob="Desert Bandit"},
    [60] = {NPC="Quest Giver", QuestId=6, isBoss=false, mob="Snow Bandit"},
    [75] = {NPC="Quest Giver", QuestId=7, isBoss=false, mob="Rebel Soldier"},
    [90] = {NPC="Quest Giver", QuestId=8, isBoss=false, mob="Military Soldier"},
    -- Second Sea
    [700]  = {NPC="Quest Giver", QuestId=1, isBoss=false, mob="Raider"},
    [775]  = {NPC="Quest Giver", QuestId=2, isBoss=false, mob="Mercenary"},
    [850]  = {NPC="Quest Giver", QuestId=3, isBoss=false, mob="Swan Pirate"},
    [925]  = {NPC="Quest Giver", QuestId=4, isBoss=false, mob="Magma Ninja"},
    [1000] = {NPC="Quest Giver", QuestId=5, isBoss=false, mob="Living Zombie"},
    -- Third Sea
    [1500] = {NPC="Quest Giver", QuestId=1, isBoss=false, mob="Pirate Millionaire"},
    [1625] = {NPC="Quest Giver", QuestId=2, isBoss=false, mob="Pistol Billionaire"},
    [1750] = {NPC="Quest Giver", QuestId=3, isBoss=false, mob="Dragon Crew Warrior"},
    [1900] = {NPC="Quest Giver", QuestId=4, isBoss=false, mob="Reborn Skeleton"},
}

local BossKw = {"boss","captain","lord","king","queen","admiral","commander","warlord","elite"}
local function IsBossQ(n,q)
    local t = (tostring(n).." "..tostring(q)):lower()
    for _,kw in ipairs(BossKw) do if t:find(kw,1,true) then return true end end
    return false
end

local function GetLatestQuest()
    local lvl = GetLevel(); local best, bl = nil, 0
    for l,d in pairs(QuestData) do
        if l<=lvl and l>bl and not (d.isBoss or IsBossQ(d.NPC,d.QuestName)) then bl,best=l,d end
    end
    return best
end

local function GetQuestMobName()
    if _G.AKH.QuestMobName then return _G.AKH.QuestMobName end
    -- Пытаемся из GUI
    local gui = LocalPlayer.PlayerGui:FindFirstChild("Quest")
    if gui then
        for _,v in pairs(gui:GetDescendants()) do
            if v:IsA("TextLabel") or v:IsA("TextButton") then
                local m = v.Text:match("Defeat%s+%d+%s+([%w%s]+)")
                       or v.Text:match("Победи%s+%d+%s+([%w%s]+)")
                if m then return m:gsub("%s+$","") end
            end
        end
    end
    -- Fallback — из таблицы по уровню
    local q = GetLatestQuest()
    return q and q.mob or nil
end

local function IsQuestMob(n,q)
    if not q then return false end
    return n:lower():find(q:lower(),1,true) ~= nil
end

local function BringMobsUnder()
    local _,hrp = GetChar(); if not hrp then return end
    local qm = GetQuestMobName()
    local base = CFrame.new(hrp.Position - Vector3.new(0,_G.AKH.MobSafeDistance,0))
    local i = 0
    local ef = workspace:FindFirstChild("Enemies"); if not ef then return end
    for _,mob in pairs(ef:GetChildren()) do
        if mob:FindFirstChild("Humanoid") and mob.Humanoid.Health>0
           and mob:FindFirstChild("HumanoidRootPart") and IsQuestMob(mob.Name,qm) then
            local ang = (i/10)*math.pi*2
            local off = Vector3.new(math.cos(ang)*_G.AKH.MobClusterRadius,0,math.sin(ang)*_G.AKH.MobClusterRadius)
            mob.HumanoidRootPart.CFrame = base + off
            mob.HumanoidRootPart.Velocity = Vector3.zero
            pcall(function()
                mob.Humanoid.PlatformStand = true
                mob.Humanoid:ChangeState(Enum.HumanoidStateType.Physics)
            end)
            i = i + 1
        end
    end
end

local function GetTargetsInRange()
    local _,hrp = GetChar(); if not hrp then return {} end
    local t = {}
    local ef = workspace:FindFirstChild("Enemies"); if not ef then return t end
    for _,mob in pairs(ef:GetChildren()) do
        if mob:FindFirstChild("Humanoid") and mob.Humanoid.Health>0
           and mob:FindFirstChild("HumanoidRootPart")
           and (mob.HumanoidRootPart.Position-hrp.Position).Magnitude <= _G.AKH.AuraRange then
            table.insert(t,mob)
        end
    end
    return t
end

-- ===== MOVEMENT =====
SafeLoop("Speed", function()
    if not _G.AKH.SpeedHack then return end
    local _,_,hum = GetChar(); if hum then hum.WalkSpeed = _G.AKH.SpeedValue end
end, 1)

local flyBV, flyBG
local function StartFly()
    local _,hrp = GetChar(); if not hrp then return end
    if flyBV then flyBV:Destroy() end
    if flyBG then flyBG:Destroy() end
    flyBV = Instance.new("BodyVelocity", hrp); flyBV.MaxForce = Vector3.new(1e5,1e5,1e5)
    flyBV.Velocity = Vector3.new(0, _G.AKH.FlySpeed, 0)
    flyBG = Instance.new("BodyGyro", hrp); flyBG.MaxTorque = Vector3.new(1e5,1e5,1e5); flyBG.P = 1000
end
local function StopFly()
    if flyBV then flyBV:Destroy() flyBV=nil end
    if flyBG then flyBG:Destroy() flyBG=nil end
end

SafeLoop("Fly", function()
    if not _G.AKH.Fly then return end
    local _, hrp = GetChar(); if not hrp then return end
    if not flyBV or not flyBV.Parent then StartFly() end
    if flyBV then flyBV.Velocity = Vector3.new(0, _G.AKH.FlySpeed, 0) end
end, 0.1)

UserInput.JumpRequest:Connect(function()
    if _G.AKH.InfiniteJump then
        local _,_,hum = GetChar(); if hum then
            pcall(function() hum:ChangeState(Enum.HumanoidStateType.Jumping) end)
        end
    end
end)

SafeLoop("Noclip", function()
    if not _G.AKH.Noclip then return end
    local c = LocalPlayer.Character; if not c then return end
    for _,p in pairs(c:GetDescendants()) do
        if p:IsA("BasePart") and p.CanCollide then p.CanCollide = false end
    end
end, 0.2)

-- Tap TP (безопасно через TouchTap)
local lastTap = 0
UserInput.TouchTap:Connect(function(touches)
    if not _G.AKH.ClickTP then return end
    if tick() - lastTap < 0.4 then return end
    lastTap = tick()
    local touch = touches and touches[1]
    if not touch then return end
    pcall(function()
        local ray = Camera:ViewportPointToRay(touch.Position.X, touch.Position.Y)
        local rp = RaycastParams.new()
        rp.FilterType = Enum.RaycastFilterType.Exclude
        rp.FilterDescendantsInstances = {LocalPlayer.Character}
        local result = workspace:Raycast(ray.Origin, ray.Direction * 500, rp)
        if result then
            local _, hrp = GetChar()
            if hrp then hrp.CFrame = CFrame.new(result.Position + Vector3.new(0,3,0)) end
        end
    end)
end)

SafeLoop("WaterWalk", function()
    if not _G.AKH.WaterWalk then return end
    local _,_,hum = GetChar()
    if hum then pcall(function() hum:SetStateEnabled(Enum.HumanoidStateType.Swimming, false) end) end
end, 1)

-- ===== COMBAT =====
if hookmetamethod then
    pcall(function()
        local old
        old = hookmetamethod(game, "__namecall", function(self, ...)
            if _G.AKH.SilentAim and getnamecallmethod()=="FireServer" then
                local args = {...}
                if self.Name=="CommF_" or tostring(self):find("Combat") then
                    local _,hrp = GetChar()
                    if hrp then
                        local cl, dist = nil, math.huge
                        local ef = workspace:FindFirstChild("Enemies")
                        if ef then
                            for _,mob in pairs(ef:GetChildren()) do
                                if mob:FindFirstChild("Humanoid") and mob.Humanoid.Health>0
                                   and mob:FindFirstChild("Head") then
                                    local dd = (mob.Head.Position-hrp.Position).Magnitude
                                    if dd < dist then dist,cl = dd,mob end
                                end
                            end
                        end
                        if cl and cl:FindFirstChild("Head") then
                            for k,a in ipairs(args) do
                                if typeof(a)=="CFrame" then
                                    args[k] = CFrame.new(cl.Head.Position)
                                end
                            end
                            return old(self, unpack(args))
                        end
                    end
                end
            end
            return old(self, ...)
        end)
    end)
end

SafeLoop("Hitbox", function()
    if not _G.AKH.HitboxExpand then return end
    local ef = workspace:FindFirstChild("Enemies"); if not ef then return end
    for _,mob in pairs(ef:GetChildren()) do
        if mob:FindFirstChild("Humanoid") and mob.Humanoid.Health>0 then
            for _,p in pairs(mob:GetDescendants()) do
  
