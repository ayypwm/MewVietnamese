-- script build an island
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Window = Rayfield:CreateWindow({
	Name = "ayypwm hub| Build an Island V1",
	LoadingTitle = "ayypwm Hub",
	LoadingSubtitle = "script loading",
	ConfigurationSaving = {
		Enabled = true,
		FolderName = "ayypwmhubIsland", -- lưu trong workspace local
		FileName = "BuildConfig"
	},
	KeySystem = false -- không cần key
})

-- TAB 1: INFO
local Tab_Info = Window:CreateTab("📄 Info Script", 4483362458)
Tab_Info:CreateParagraph({
	Title = "ayypwm Hub",
	Content = "Script nokey"
})

Tab_Info:CreateButton({
	Name = "❤️ Donate Robux (Ủng hộ 37R$)",
	Callback = function()
		local MarketplaceService = game:GetService("MarketplaceService")
		local Players = game:GetService("Players")
		local player = Players.LocalPlayer
		MarketplaceService:PromptGamePassPurchase(player, 1284506367)
	end,
})

Tab_Info:CreateParagraph({
    Title = "Ủng hộ ",
    Content = "Nếu bạn thấy script hữu ích, hãy ủng hộ mình bằng cách vượt qua link bên dưới. Cảm ơn bạn rất nhiều <3"
})

Tab_Info:CreateButton({
    Name = "Ủng hộ qua Linkvertise",
    Callback = function()
        setclipboard("https://link-hub.net/1365790/jeTrfosHqFEP") -- thay bằng link của bạn
        warn("Đã copy link ủng hộ vào clipboard!")
    end
})

-- TAB 2: FARM
local Tab_Farm = Window:CreateTab("⚒️ Farm", 4483362458)

-- 🌾 Auto Farm toàn đảo có chọn người chơi

local SelectedPlayer = game.Players.LocalPlayer.Name
local AutoFarm = false
local PlayerList = {}
local Dropdown_FarmTarget = nil

-- 🔧 Hàm tìm tài nguyên trong đảo người chơi
local function getResources(playerName)
	local plots = game:GetService("Workspace"):WaitForChild("Plots")
	local targetPlot = plots:FindFirstChild(playerName)
	if targetPlot then
		return targetPlot:FindFirstChild("Resources")
	end
	return nil
end

-- 🔁 Nút cập nhật danh sách người chơi
Tab_Farm:CreateButton({
	Name = "Cập nhật người chơi",
	Callback = function()
		PlayerList = {}
		for _, plr in ipairs(game.Players:GetPlayers()) do
			table.insert(PlayerList, plr.Name)
		end
		if Dropdown_FarmTarget then
			Dropdown_FarmTarget:UpdateOptions(PlayerList)
			print("Danh sách người chơi đã được cập nhật")
		end
	end,
})

-- 🏝️ Dropdown chọn đảo cần farm
Dropdown_FarmTarget = Tab_Farm:CreateDropdown({
	Name = "Chọn đảo để farm",
	Options = PlayerList,
	CurrentOption = SelectedPlayer,
	Flag = "FarmTarget",
	Callback = function(option)
		SelectedPlayer = option
		print("Đã chọn đảo:", SelectedPlayer)
	end,
})

-- ⚒️ Auto Farm toàn đảo
Tab_Farm:CreateToggle({
	Name = " Auto Farm toàn đảo (máy yếu có thể tuột còn 0-10 fps)",
	CurrentValue = false,
	Flag = "AutoFarm",
	Callback = function(state)
		AutoFarm = state
		if AutoFarm then
			print("Đang Auto Farm đảo (máy yếu có thể tuột còn 0-10 fps):", SelectedPlayer)
			task.spawn(function()
				while AutoFarm do
					local res = getResources(SelectedPlayer)
					if res then
						for _, r in ipairs(res:GetChildren()) do
							if r:IsA("Model") then
								task.spawn(function()
									game:GetService("ReplicatedStorage")
										:WaitForChild("Communication")
										:WaitForChild("HitResource")
										:FireServer(r)
								end)
							end
						end
					end
					task.wait(0.5)
				end
			end)
		else
			print("Auto Farm đã tắt (máy yếu có thể tuột còn 0-10 fps)")
		end
	end,
})

-- TAB 3: CRAFT
local Tab_Craft = Window:CreateTab("🛠️ Craft", 4483362458)
Tab_Craft:CreateParagraph({
	Title = "",
	Content = "beta"
})

-- Biến lưu cài đặt
local selected_crafter = nil
local auto_craft_enabled = false
local craft_delay = 2
local crafter_dropdown = nil -- tham chiếu dropdown để cập nhật danh sách

-- Hàm lấy danh sách crafter
local function getCrafters()
	local list = {}
	for _, c in pairs(game:GetService("Workspace").Plots[game.Players.LocalPlayer.Name]:GetDescendants()) do
		if c.Name == "Crafter" then
			local attachment = c:FindFirstChildOfClass("Attachment")
			if attachment then
				table.insert(list, c:GetFullName())
			end
		end
	end
	return list
end

-- Dropdown chọn Crafter
crafter_dropdown = Tab_Craft:CreateDropdown({
	Name = " Chọn vật phẩm Craft",
	Options = getCrafters(),
	CurrentOption = "",
	Flag = "SelectCraftTarget",
	Callback = function(value)
		selected_crafter = value
		print("Đã chọn:", value)
	end,
})

-- Nút cập nhật danh sách craft
Tab_Craft:CreateButton({
	Name = "Cập nhật danh sách nơi Craft",
	Callback = function()
		local new_list = getCrafters()
		crafter_dropdown:Refresh(new_list)
		print("Đã làm mới danh sách Crafter")
	end,
})

-- Thanh chỉnh delay
Tab_Craft:CreateSlider({
	Name = " Delay mỗi lần Craft (giây)",
	Range = {1, 20},
	Increment = 1,
	CurrentValue = 2,
	Flag = "CraftDelay",
	Callback = function(Value)
		craft_delay = Value
		print("🛠️ Delay Craft:", Value .. " giây")
	end,
})

-- Nút bật Auto Craft
Tab_Craft:CreateToggle({
	Name = "Auto Craft",
	CurrentValue = false,
	Flag = "AutoCraft",
	Callback = function(Value)
		auto_craft_enabled = Value
		if Value then
			print("Auto Craft đã bật")
			task.spawn(function()
				while auto_craft_enabled and selected_crafter do
					local crafter = nil
					pcall(function()
						crafter = loadstring("return " .. selected_crafter)()
					end)
					if crafter then
						local attachment = crafter:FindFirstChildOfClass("Attachment")
						if attachment then
							game:GetService("ReplicatedStorage"):WaitForChild("Communication"):WaitForChild("Craft"):FireServer(attachment)
							print("🧪 Craft tại:", crafter:GetFullName())
						end
					end
					task.wait(craft_delay)
				end
			end)
		else
			print("Auto Craft đã tắt")
		end
	end
})

-- TAB 4: SHOP
local Tab_Shop = Window:CreateTab("🛒 Shop", 4483362458)
Tab_Shop:CreateParagraph({
	Title = "beta",
	Content = "nói chuyện với npc merchant và mua vài món hàng để script load vật phẩm để mua"
})

-- ⏰ Hiển thị thời gian reset stock (đếm ngược)
local timerLabel

-- Thêm nhãn ban đầu
timerLabel = Tab_Shop:CreateParagraph({
	Title = "Reset shop sau...",
	Content = "Đang tải..."
})

-- Cập nhật nội dung mỗi 1 giây
task.spawn(function()
	while true do
		local gui = game.Players.LocalPlayer:FindFirstChild("PlayerGui")
		if gui then
			local timer = gui:FindFirstChild("Main")
				and gui.Main:FindFirstChild("Menus")
				and gui.Main.Menus:FindFirstChild("Merchant")
				and gui.Main.Menus.Merchant.Inner
				and gui.Main.Menus.Merchant.Inner:FindFirstChild("Timer")

			if timer and timer:IsA("TextLabel") then
				timerLabel:Set({
					Title = "Reset shop merchent sau:",
					Content = timer.Text
				})
			end
		end
		task.wait(1)
	end
end)

local selectedItems = {}
local shownItems = {}
local allItems = {}
local autoBuying = false

-- 🧠 Tự động quét danh sách item trong shop
task.spawn(function()
	while true do
		local gui = game.Players.LocalPlayer:FindFirstChild("PlayerGui")
		if gui and gui:FindFirstChild("Main") and gui.Main:FindFirstChild("Menus") then
			local merchant = gui.Main.Menus:FindFirstChild("Merchant")
			if merchant and merchant.Visible and merchant.Inner then
				local frame = merchant.Inner:FindFirstChild("ScrollingFrame")
				if frame and frame:FindFirstChild("Hold") then
					for _, item in ipairs(frame.Hold:GetChildren()) do
						if item:IsA("Frame") and item.Name ~= "Example" and not table.find(shownItems, item.Name) then
							table.insert(allItems, item.Name)
							table.insert(shownItems, item.Name)

							Tab_Shop:CreateToggle({
								Name = item.Name,
								CurrentValue = false,
								Callback = function(state)
									if state then
										table.insert(selectedItems, item.Name)
									else
										for i, v in ipairs(selectedItems) do
											if v == item.Name then
												table.remove(selectedItems, i)
												break
											end
										end
									end
								end
							})
						end
					end
				end
			end
		end
		task.wait(5)
	end
end)

-- ♻️ Auto Buy Toggle
Tab_Shop:CreateToggle({
	Name = "Auto Buy các vật phẩm đã chọn",
	CurrentValue = false,
	Callback = function(state)
		autoBuying = state
		if autoBuying then
			task.spawn(function()
				while autoBuying do
					for _, item in ipairs(selectedItems) do
						local args = {
							[1] = item,
							[2] = false
						}
						game:GetService("ReplicatedStorage").Communication.BuyFromMerchant:FireServer(unpack(args))
						task.wait(0.25)
					end
					task.wait(1)
				end
			end)
		end
	end
})

-- ⏰ Nhãn reset
Tab_Shop:CreateParagraph({
	Title = "Shop Reset",
	Content = "Danh sách cập nhật tự động khi mở shop"
})

-- TAB 5: SELL
local Tab_Sell = Window:CreateTab("💰 Sell", 4483362458)
Tab_Sell:CreateParagraph({
	Title = "",
	Content = "beta."
})

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local plr = Players.LocalPlayer

local AutoSell = false

Tab_Sell:CreateToggle({
	Name = "Auto Sell (mỗi 3 giây)",
	CurrentValue = false,
	Flag = "AutoSell",
	Callback = function(Value)
		AutoSell = Value
		if AutoSell then
			task.spawn(function()
				while AutoSell do
					for _, item in pairs(plr.Backpack:GetChildren()) do
						if item:GetAttribute("Sellable") then
							local hash = item:GetAttribute("Hash")
							if hash then
								ReplicatedStorage.Communication.SellToMerchant:FireServer(false, {hash})
								print("💰 Đã bán:", item.Name)
							end
						end
					end
					task.wait(3)
				end
			end)
		end
	end
})

-- TAB 6: COLLECT
local Tab_Collect = Window:CreateTab("📦 Collect", 4483362458)
Tab_Collect:CreateParagraph({
	Title = "tính năng thu hoạch cá sẽ được cập nhật sau",
	Content = ""
})

-- Auto Harvest trái cây
Tab_Collect:CreateToggle({
	Name = "Auto Harvest Plants",
	CurrentValue = false,
	Flag = "AutoHarvest",
	Callback = function(Value)
		getgenv().CollectHarvest = Value
		task.spawn(function()
			while getgenv().CollectHarvest do
				local plot = game:GetService("Workspace"):WaitForChild("Plots"):FindFirstChild(game.Players.LocalPlayer.Name)
				if plot and plot:FindFirstChild("Plants") then
					for _, crop in pairs(plot.Plants:GetChildren()) do
						game:GetService("ReplicatedStorage"):WaitForChild("Communication"):WaitForChild("Harvest"):FireServer(crop.Name)
					end
				end
				task.wait()
			end
		end)
	end
})

-- Auto thu hoạch mật ong
Tab_Collect:CreateToggle({
	Name = "Auto Collect Honey",
	CurrentValue = false,
	Flag = "AutoHoney",
	Callback = function(Value)
		getgenv().CollectHoney = Value
		task.spawn(function()
			while getgenv().CollectHoney do
				local land = game:GetService("Workspace"):WaitForChild("Plots"):FindFirstChild(game.Players.LocalPlayer.Name):FindFirstChild("Land")
				for _, spot in ipairs(land:GetDescendants()) do
					if spot:IsA("Model") and spot.Name:match("Spot") then
						game:GetService("ReplicatedStorage"):WaitForChild("Communication"):WaitForChild("Hive"):FireServer(spot.Parent.Name, spot.Name, 2)
					end
				end
				task.wait()
			end
		end)
	end
})

-- Auto thu hoạch vàng
Tab_Collect:CreateToggle({
	Name = " Auto Collect Gold",
	CurrentValue = false,
	Flag = "AutoGold",
	Callback = function(Value)
		getgenv().CollectGold = Value
		task.spawn(function()
			while getgenv().CollectGold do
				local land = game:GetService("Workspace"):WaitForChild("Plots"):FindFirstChild(game.Players.LocalPlayer.Name):FindFirstChild("Land")
				for _, mine in pairs(land:GetDescendants()) do
					if mine:IsA("Model") and mine.Name == "GoldMineModel" then
						game:GetService("ReplicatedStorage"):WaitForChild("Communication"):WaitForChild("Goldmine"):FireServer(mine.Parent.Name, 2)
					end
				end
				task.wait()
			end
		end)
	end
})

-- TAB 7: EVENTS
local Tab_Events = Window:CreateTab("🎉 Events", 4483362458)
Tab_Events:CreateParagraph({
	Title = "Chưa có tính năng",
	Content = "Tính năng Events sẽ được cập nhật sau:(("
})

-- TAB 8: MISC
local Tab_Misc = Window:CreateTab("🧩 Misc", 4483362458)
Tab_Misc:CreateParagraph({
	Title = "",
	Content = "beta"
})

local AntiAFKConnection = nil

Tab_Misc:CreateToggle({
	Name = "Anti-AFK",
	CurrentValue = false,
	Flag = "AntiAFK",
	Callback = function(Value)
		if Value then
			local vu = game:GetService("VirtualUser")
			AntiAFKConnection = game:GetService("Players").LocalPlayer.Idled:Connect(function()
				vu:CaptureController()
				vu:ClickButton2(Vector2.new())
				print("Đã giả lập thao tác tránh AFK")
			end)
			print("Anti-AFK Đã bật")
		else
			if AntiAFKConnection then
				AntiAFKConnection:Disconnect()
				AntiAFKConnection = nil
				print("Anti-AFK Đã tắt")
			end
		end
	end,
})

-- 🏃 Tăng tốc độ di chuyển
Tab_Misc:CreateSlider({
	Name = "Speed Nhân Vật",
	Range = {16, 50},
	Increment = 1,
	CurrentValue = 16,
	Flag = "WalkSpeed",
	Callback = function(Value)
		game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
	end,
})

-- ⬆️ Nhảy cao
Tab_Misc:CreateSlider({
	Name = "Độ Nhảy Cao (đang lỗi:D)",
	Range = {50, 150},
	Increment = 5,
	CurrentValue = 50,
	Flag = "JumpPower",
	Callback = function(Value)
		game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
	end,
})

-- 🔁 Rejoin server
Tab_Misc:CreateButton({
	Name = "Rejoin Server (chỉ hoạt động với server công cộng)",
	Callback = function()
		local ts = game:GetService("TeleportService")
		local plr = game.Players.LocalPlayer
		ts:Teleport(game.PlaceId, plr)
	end,
})
