local _={};local __=function(...)return string.char(...)end;local ___=table.concat
local ____=game;local _____=____.GetService
local ______=_____(____,"Players")
local _______=_____(____,"ReplicatedStorage")
local ________=_____(____,"RunService")
local _________=_____(____,"Workspace")

local __________=___({114,98,120,97,115,115,101,116,105,100,58,47,47,56,57,55,52,54,57,51,56,54,50,51,50,48,55})
local ___________=___({114,98,120,97,115,115,101,116,105,100,58,47,47,55,54,54,57,51,51,55,50,51,48,55,57,53,56})

local function ____________(c)
	local r=c:FindFirstChild("HumanoidRootPart")
	if not r then return end
	for _,s in ipairs(r:GetChildren()) do
		if s:IsA("Sound") then
			if s.Name=="Running" then
				s.SoundId=__________;s.Volume=0.3
			elseif s.Name=="Jumping" then
				s.SoundId=___________;s.Volume=0.3
			end
		end
	end
end

for _,v in ipairs(_________:GetChildren()) do
	if v:FindFirstChild("HumanoidRootPart") then
		____________(v)
	end
end

_________.ChildAdded:Connect(function(c)
	if c:WaitForChild("HumanoidRootPart",3) then
		____________(c)
	end
end)

local _____________=______(____,"Players").LocalPlayer
local ______________=Instance.new("ScreenGui",___________:WaitForChild("PlayerGui"))
______________.Name="FTF_MASTER_GUI"
______________.ResetOnSpawn=false

local _______________=false
local ________________={}

local function _________________(o,c)
	if o:FindFirstChild("ESP_HIGHLIGHT") then return end
	local h=Instance.new("Highlight")
	h.Name="ESP_HIGHLIGHT"
	h.FillColor=c
	h.OutlineColor=c
	h.FillTransparency=0.6
	h.OutlineTransparency=0.1
	h.Parent=o
	table.insert(________________,o)
end

local function __________________()
	for _,p in ipairs(______ :GetPlayers()) do
		if p~=___________ and p.Character then
			_________________(p.Character,Color3.fromRGB(255,0,0))
		end
	end
	for _,m in ipairs(_________:GetDescendants()) do
		if m:IsA("Model") then
			local n=m.Name:lower()
			if n:find("door") or n:find("exit") then
				local a=false
				if m:FindFirstChild("IsOpen") and m.IsOpen.Value then a=true end
				if m:FindFirstChild("Open") and m.Open.Value then a=true end
				if m:FindFirstChild("Opened") and m.Opened.Value then a=true end
				local c=a and Color3.fromRGB(50,255,50) or Color3.fromRGB(255,220,0)
				_________________(m,c)
			end
		end
	end
end

local function ___________________()
	for _,o in ipairs(________________) do
		if o:FindFirstChild("ESP_HIGHLIGHT") then
			o.ESP_HIGHLIGHT:Destroy()
		end
	end
	________________={}
end

local ____________________=false

local function _____________________(p)
	if p>=1 then return Color3.fromRGB(0,255,0) end
	if p<0.5 then
		return Color3.fromRGB(255,255*p*2,255*p*2)
	else
		return Color3.fromRGB(255,255-((p-0.5)*2*255),0)
	end
end

local function ______________________(pc)
	if pc:FindFirstChild("ProgressBar") then return end
	local g=Instance.new("BillboardGui",pc)
	g.Name="ProgressBar"
	g.Size=UDim2.new(0,160,0,16)
	g.StudsOffset=Vector3.new(0,2.6,0)
	g.AlwaysOnTop=true
	g.Enabled=____________________
	local bg=Instance.new("Frame",g)
	bg.Size=UDim2.new(1,0,1,0)
	bg.BackgroundColor3=Color3.fromRGB(0,0,0)
	local b=Instance.new("Frame",bg)
	b.Size=UDim2.new(0,0,1,0)
	local t=Instance.new("TextLabel",bg)
	t.Size=UDim2.new(1,0,1,0)
	t.BackgroundTransparency=1
	t.TextScaled=true
	t.Font=Enum.Font.SciFi
	t.TextColor3=Color3.new(1,1,1)
	local h=Instance.new("Highlight",pc)
	h.Name="ComputerHighlight"
	h.Enabled=____________________
	local s=0
	__________.Heartbeat:Connect(function()
		if not ____________________ then return end
		local hi=0
		for _,p in ipairs(pc:GetChildren()) do
			if p:IsA("BasePart") and p.Name:match("ComputerTrigger") then
				for _,tp in ipairs(p:GetTouchingParts()) do
					local pl=______ :GetPlayerFromCharacter(tp.Parent)
					if pl then
						local m=pl:FindFirstChild("TempPlayerStatsModule")
						if m and not m.Ragdoll.Value then
							hi=math.max(hi,m.ActionProgress.Value)
						end
					end
				end
			end
		end
		s=math.max(s,hi)
		b.Size=UDim2.new(s,0,1,0)
		b.BackgroundColor3=_____________________(s)
		if s>=1 then
			t.Text="COMPLETED"
			h.FillColor=Color3.fromRGB(0,255,0)
		else
			t.Text=math.floor(s*100).."%"
		end
	end)
end

task.spawn(function()
	local lm
	while true do
		local cm=tostring(_________.CurrentMap.Value)
		if cm~=lm then
			lm=cm
			local mp=_________:FindFirstChild(cm)
			if mp then
				for _,o in ipairs(mp:GetChildren()) do
					if o.Name=="ComputerTable" then
						______________________(o)
					end
				end
			end
		end
		task.wait(1)
	end
end)

local _____________________=false
local ______________________=Instance.new("TextButton",______________)
______________________.Size=UDim2.new(0,120,0,60)
______________________.Position=UDim2.new(0,10,0,10)
______________________.BackgroundColor3=Color3.fromRGB(25,25,25)
______________________.BorderSizePixel=0
______________________.Text="OFF"
______________________.TextScaled=true
______________________.Font=Enum.Font.GothamBold
______________________.TextColor3=Color3.fromRGB(255,80,80)

______________________.MouseButton1Click:Connect(function()
	_____________________ = not _____________________
	______________ = _____________________
	____________________ = _____________________
	if _____________________ then
		______________________.Text="ON"
		______________________.TextColor3=Color3.fromRGB(80,255,80)
		__________________()
	else
		______________________.Text="OFF"
		______________________.TextColor3=Color3.fromRGB(255,80,80)
		___________________()
	end
	for _,d in ipairs(_________:GetDescendants()) do
		if d:IsA("BillboardGui") and d.Name=="ProgressBar" then
			d.Enabled=____________________
		elseif d:IsA("Highlight") and d.Name=="ComputerHighlight" then
			d.Enabled=____________________
		end
	end
end)

print("✔")
