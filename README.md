
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "XNONHUB | GrowAGarden",
    SubTitle = "By 001x",
    TabWidth = 160,
    Size = UDim2.fromOffset(550, 350),
    Acrylic = false,
    Theme = "Amethyst",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Log = Window:AddTab({ Title = "Log", Icon = "crown" }),
    Main = Window:AddTab({ Title = "Main", Icon = "code" }),
    Shop = Window:AddTab({ Title = "Shop", Icon = "box" }),
    Plr = Window:AddTab({ Title = "Players", Icon = "user" }),
    TP = Window:AddTab({ Title = "Teleport", Icon = "map-pin" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options


local Plr = game:GetService("Players")
local LocalPlr = Plr.LocalPlayer
local StarterGui = game:GetService("StarterGui")


do
    pcall(function()
    StarterGui:SetCore("SendNotification", {
                Title = "XNON HUB",
                Icon = "rbxassetid://79996080860226",
                Text = "ยินดีต้อนรับ อย่าลืมเข้าดิสคอร์ดด้วยน้า",
                Duration = 10,
    })

    
    Tabs.Plr:AddSection("[ Speed / ความเร็ว ]")

    local Input = Tabs.Plr:AddInput("Input", {
        Title = "WalkSpeed",
        Default = "16",
        Placeholder = "",
        Numeric = false, -- Only allows numbers
        Finished = false, -- Only calls callback when you press enter
        Callback = function(Value)
            Speed = Value
        end
    })

    Input:OnChanged(function()
        --print("Set Speed :", Input.Value)
    end)
    
    Tabs.Plr:AddButton({
        Title = "Set WalkSpeed",
        Description = "กดเพื่อเปลี่ยนความเร็ว",
        Callback = function()
            LocalPlr.Character.Humanoid.WalkSpeed = Speed
        end
    })

    Tabs.Plr:AddSection("[ Jump / กระโดด ]")

    local Input = Tabs.Plr:AddInput("Input", {
        Title = "JumpPower",
        Default = "50",
        Placeholder = "",
        Numeric = false,
        Finished = false,
        Callback = function(Value)
            Jump = Value
        end
    })

    Input:OnChanged(function()
        --print("Set Speed :", Input.Value)
    end)

    Tabs.Plr:AddButton({
        Title = "Set JumpPower",
        Description = "กดเพื่อเปลี่ยนพลังกระโดด",
        Callback = function()
            LocalPlr.Character.Humanoid.JumpPower = Jump
        end
    })

    

    Tabs.Log:AddSection("Link")

    Tabs.Log:AddButton({
        Title = "Discord Invite",
        Description = "กดเพื่อคัดลอกลิ้งค์ดิสคอร์ด",
        Callback = function()
            setclipboard("https://discord.gg/ZENSyVq8KD")
            StarterGui:SetCore("SendNotification", {
                Title = "XNON HUB",
                Icon = "rbxassetid://79996080860226",
                Text = "คัดลอกลิ้งแล้ว",
                Duration = 5,
            })
        end
    })
    end)
end

Tabs.Log:AddButton({
    Title = "Anti AFK",
    Description = "กันหลุด20นาที",
    Callback = function()
        local Players = game:GetService("Players")
        local StarterGui = game:GetService("StarterGui")
        local VirtualUser = game:GetService("VirtualUser")

        Players.LocalPlayer.Idled:Connect(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end)

        StarterGui:SetCore("SendNotification", {
            Title = "XNON HUB",
            Text = "เปิดใช้งาน Anti AFK แล้ว",
            Duration = 5,
        })
    end
})


SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({})
InterfaceManager:SetFolder("XNON HUB")
SaveManager:SetFolder("XNON HUB/specific-game")
InterfaceManager:BuildInterfaceSection(Tabs.Settings)
-- SaveManager:BuildConfigSection(Tabs.Settings)
Window:SelectTab(1)


Tabs.Settings:AddButton({
    Title = "Copy JobId",
    Description = "",
    Callback = function()
        setclipboard(tostring(game.JobId))
        StarterGui:SetCore("SendNotification", {
                Title = "XNON HUB",
                Icon = "rbxassetid://79996080860226",
                Text = "คัดลอกไอดีเชิฟเวอร์แล้ว",
                Duration = 5,
        })
    end
})

local Input = Tabs.Settings:AddInput("Input", {
    Title = "JobId",
    Default = "",
    Placeholder = "Enter JobId",
    Numeric = false,
    Finished = false, 
    Callback = function(Value)
        JobId = Value
    end
})

Input:OnChanged(function()
    --print("TP JobId :", Input.Value)
end)

Tabs.Settings:AddButton({
    Title = "Tepeport To JobId",
    Description = "",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, JobId)
    end
})
task.wait(1)
local CoreGui = game:GetService("CoreGui")
local logoGui = Instance.new("ScreenGui")

logoGui.Name = "XNONHUB"
logoGui.ResetOnSpawn = false
logoGui.Parent = CoreGui.ScreenGui

local logoButton = Instance.new("ImageButton")
logoButton.Name = "LogoButton"
logoButton.Size = UDim2.new(0, 55, 0, 55)
logoButton.Position = UDim2.new(0, 65, 0, 50)
logoButton.BackgroundTransparency = 1
logoButton.Image = "rbxassetid://112379455354342"
logoButton.Parent = logoGui
logoButton.ZIndex = 900
logoButton.MouseButton1Click:Connect(function()
    CoreGui.ScreenGui:GetChildren()[2].Visible = not CoreGui.ScreenGui:GetChildren()[2].Visible
end)

function dragify(Frame, object)
    local dragToggle = false
    local dragSpeed = 0.1
    local dragInput = nil
    local dragStart = nil
    local startPos = nil

    local function updateInput(input)
        local Delta = input.Position - dragStart
        local Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + Delta.X, startPos.Y.Scale, startPos.Y.Offset + Delta.Y)
        game:GetService("TweenService"):Create(object, TweenInfo.new(dragSpeed), {Position = Position}):Play()
    end

    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragToggle = true
            dragStart = input.Position
            startPos = object.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragToggle = false
                end
            end)
        end
    end)

    Frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragToggle then
            updateInput(input)
        end
    end)
end

dragify(logoButton, logoButton)


local ReplicatedStorage = game:GetService("ReplicatedStorage")
local BuyRemote = ReplicatedStorage:WaitForChild("GameEvents"):WaitForChild("BuySeedStock")

local Seeds = {
    "ALL",
    "Carrot", "Strawberry", "Blueberry", "Orange Tulip", "Tomato",
    "Daffodil", "Corn", "Watermelon", "Pumpkin", "Bamboo",
    "Coconut", "Dragon Fruit", "Cactus", "Mango", "Grape",
    "Pepper", "Mushroom", "Cacao", "Beanstalk", "Ember Lily",
    "Sugar Apple", "Burning Bud"
}

local selectedSeed = nil
local buySeedLoop = false

Tabs.Shop:AddSection("Auto buy")

local Dropdown = Tabs.Shop:AddDropdown("Dropdown", {
    Title = "Select Seed",
    Values = Seeds,
    Multi = false,
    Default = "เลือกเมล็ด",
})

Dropdown:OnChanged(function(Value)
    selectedSeed = Value
end)

Tabs.Shop:AddToggle("BuySeedToggle", {
    Title = "Buy Seed",
    Default = false,
    Callback = function(State)
        buySeedLoop = State

        if buySeedLoop then
            task.spawn(function()
                while buySeedLoop and task.wait(0.1) do
                    if not selectedSeed then return end

                    if selectedSeed == "ALL" then
                        for _, seedName in ipairs(Seeds) do
                            if seedName ~= "ALL" then
                                task.spawn(function()
                                    BuyRemote:FireServer(seedName)
                                end)
                            end
                        end
                    else
                        BuyRemote:FireServer(selectedSeed)
                    end
                end
            end)
        end
    end
})


local ReplicatedStorage = game:GetService("ReplicatedStorage")
local BuyRemote = ReplicatedStorage:WaitForChild("GameEvents"):WaitForChild("BuyGearStock")

local Gears = {
    "ALL",
    "Watering Can",
    "Trowel",
    "Recall Wrench",
    "Basic Sprinkler",
    "Advanced Sprinkler",
    "Godly Sprinkler",
    "Tanning Mirror",
    "Master Sprinkler",
    "Cleaning Spray",
    "Favorite Tool",
    "Harvest Tool",
    "Friendship Pot",
    "Magnifying Glass"
}

local selectedGear = nil
local buyGearLoop = false


local Dropdown = Tabs.Shop:AddDropdown("Dropdown", {
    Title = "Select Gear",
    Values = Gears,
    Multi = false,
    Default = "เลือกอุปกรณ์",
})

Dropdown:OnChanged(function(Value)
    selectedGear = Value
end)

Tabs.Shop:AddToggle("BuyGearToggle", {
    Title = "Buy Gear",
    Default = false,
    Callback = function(State)
        buyGearLoop = State

        if buyGearLoop then
            task.spawn(function()
                while buyGearLoop and task.wait(0.1) do
                    if not selectedGear then return end

                    if selectedGear == "ALL" then
                        for _, gearName in ipairs(Gears) do
                            if gearName ~= "ALL" then
                                task.spawn(function()
                                    BuyRemote:FireServer(gearName)
                                end)
                            end
                        end
                    else
                        BuyRemote:FireServer(selectedGear)
                    end
                end
            end)
        end
    end
})


local ReplicatedStorage = game:GetService("ReplicatedStorage")
local BuyEggRemote = ReplicatedStorage:WaitForChild("GameEvents"):WaitForChild("BuyPetEgg")

local eggIDs = {"ALL", "1", "2", "3"}
local selectedEgg = nil
local autoBuyEgg = false


local Dropdown = Tabs.Shop:AddDropdown("Dropdown", {
    Title = "Select Egg ID",
    Values = eggIDs,
    Multi = false,
    Default = "เลือกไข่",
})

Dropdown:OnChanged(function(Value)
    selectedEgg = Value
end)

Tabs.Shop:AddToggle("AutoBuyEggToggle", {
    Title = "Buy Egg",
    Default = false,
    Callback = function(State)
        autoBuyEgg = State

        if autoBuyEgg then
            task.spawn(function()
                while autoBuyEgg and task.wait(0.1) do
                    if not selectedEgg then return end

                    if selectedEgg == "ALL" then
                        for _, id in ipairs({1, 2, 3}) do
                            task.spawn(function()
                                BuyEggRemote:FireServer(id)
                            end)
                        end
                    else
                        local eggID = tonumber(selectedEgg)
                        if eggID then
                            BuyEggRemote:FireServer(eggID)
                        end
                    end
                end
            end)
        end
    end
})


local TPList = {
    "Gear Shop",
    "Egg Shop",
    "Dino Event",
    "Crafting"
}

local selectedTP = nil

Tabs.TP:AddSection("Teleport")

local Dropdown = Tabs.TP:AddDropdown("TPDropdown", {
    Title = "Select Teleport",
    Values = TPList,
    Multi = false,
    Default = "เลือกที่จะวาปไป",
})

Dropdown:OnChanged(function(Value)
    selectedTP = Value
end)

Tabs.TP:AddButton({
    Title = "Click To Teleport",
    Description = "กดเพื่อวาป",
    Callback = function()
        if not selectedTP then return end

        local cfTable = {
            ["Gear Shop"] = CFrame.new(-286.245758, 2.99999976, -14.0936031, -0.0132383211, -2.78008976e-08, 0.999912381, -5.67098191e-09, 1, 2.77282535e-08, -0.999912381, -5.30340971e-09, -0.0132383211),
            ["Egg Shop"] = CFrame.new(-282.938812, 2.99999976, 3.86399388, -0.0150365653, 6.56755184e-09, 0.99988693, -6.762167e-08, 1, -7.58520713e-09, -0.99988693, -6.77280809e-08, -0.0150365653),
            ["Dino Event"] = CFrame.new(-114.764969, 3.32000947, -14.5408745, 0.0133202868, -2.56502126e-08, -0.999911308, 6.97121081e-08, 1, -2.47238212e-08, 0.999911308, -6.93765969e-08, 0.0133202868),
            ["Crafting"] = CFrame.new(-284.043976, 2.99999976, -31.5364285, 0.00415412895, 8.20995112e-08, 0.999991357, -4.49235849e-08, 1, -8.19136048e-08, -0.999991357, -4.45829187e-08, 0.00415412895)
        }

        local cf = cfTable[selectedTP]
        if cf and LocalPlr and LocalPlr.Character and LocalPlr.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlr.Character.HumanoidRootPart.CFrame = cf
        end
    end
})


local GardenList = {
    "1", "2", "3", "4", "5", "6"
}

local selectedGarden = nil

Tabs.TP:AddSection("Garden")

local DropdownGarden = Tabs.TP:AddDropdown("GardenDropdown", {
    Title = "Select Garden",
    Values = GardenList,
    Multi = false,
    Default = "เลือกสวนที่จะวาปไป",
})

DropdownGarden:OnChanged(function(Value)
    selectedGarden = Value
end)

Tabs.TP:AddButton({
    Title = "Click To Teleport",
    Description = "กดเพื่อวาปไปยังสวนที่เลือก",
    Callback = function()
        if not selectedGarden then return end

        local cfTable = {
            ["1"] = CFrame.new(35.452713, 2.99999976, -61.251133, 0.999879122, -3.04142889e-09, 0.0155480979, 2.92904656e-09, 1, 7.25082128e-09, -0.0155480979, -7.20440374e-09, 0.999879122),
            ["2"] = CFrame.new(33.072197, 2.99999976, 32.93713, -0.999997556, -1.1900358e-07, 0.00220724382, -1.1895856e-07, 1, 2.05263042e-08, -0.00220724382, 2.02636823e-08, -0.999997556),
            ["3"] = CFrame.new(-99.7253876, 2.99999976, -61.126049, 0.999851346, 7.23298088e-10, 0.0172424465, 1.11310694e-09, 1, -1.064953e-07, -0.0172424465, 1.06498661e-07, 0.999851346),
            ["4"] = CFrame.new(-102.946953, 2.99999976, 32.9636688, -0.999939084, 9.31523374e-08, 0.0110350214, 9.37552471e-08, 1, 5.41187752e-08, -0.0110350214, 5.51500712e-08, -0.999939084),
            ["5"] = CFrame.new(-235.370026, 2.99999976, -61.7729034, 0.999999106, -3.36821994e-11, 0.00133441552, -8.07662964e-13, 1, 2.58464183e-08, -0.00133441552, -2.5846397e-08, 0.999999106),
            ["6"] = CFrame.new(-236.828903, 2.99999976, 33.4977112, -0.999885559, 2.4698533e-08, -0.0151285492, 2.64145932e-08, 1, -1.13232097e-07, 0.0151285492, -1.13618754e-07, -0.999885559),
        }

        local cf = cfTable[selectedGarden]
        if cf and LocalPlr and LocalPlr.Character and LocalPlr.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlr.Character.HumanoidRootPart.CFrame = cf
        end
    end
})


SaveManager:LoadAutoloadConfig()
