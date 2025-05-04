-- Jamba Hub Mobile UI - Atualizado para garantir exibição no mobile e em todos os jogos

-- Configs iniciais local player = game.Players.LocalPlayer local gui = player:FindFirstChild("PlayerGui") or Instance.new("PlayerGui", player)

if gui:FindFirstChild("JambaHub") then gui.JambaHub:Destroy() end

local ScreenGui = Instance.new("ScreenGui") ScreenGui.Name = "JambaHub" ScreenGui.ResetOnSpawn = false ScreenGui.IgnoreGuiInset = true ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling ScreenGui.Parent = gui

local MainFrame = Instance.new("Frame") MainFrame.Name = "MainFrame" MainFrame.Size = UDim2.new(0, 500, 0, 300) MainFrame.Position = UDim2.new(0.5, -250, 0.5, -150) MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25) MainFrame.Parent = ScreenGui MainFrame.Active = true MainFrame.Draggable = true

local Title = Instance.new("TextLabel") Title.Size = UDim2.new(1, -40, 0, 30) Title.Position = UDim2.new(0, 0, 0, 0) Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30) Title.Text = "Jamba Hub" Title.TextColor3 = Color3.new(1, 1, 1) Title.Font = Enum.Font.SourceSansBold Title.TextSize = 20 Title.Parent = MainFrame

local MinimizeButton = Instance.new("TextButton") MinimizeButton.Size = UDim2.new(0, 30, 0, 30) MinimizeButton.Position = UDim2.new(1, -30, 0, 0) MinimizeButton.Text = "–" MinimizeButton.TextColor3 = Color3.new(1, 1, 1) MinimizeButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60) MinimizeButton.Font = Enum.Font.SourceSansBold MinimizeButton.TextSize = 20 MinimizeButton.Parent = MainFrame

local minimized = false MinimizeButton.MouseButton1Click:Connect(function() minimized = not minimized for _, child in ipairs(MainFrame:GetChildren()) do if child ~= Title and child ~= MinimizeButton then child.Visible = not minimized end end end)

local SideMenu = Instance.new("Frame") SideMenu.Size = UDim2.new(0, 120, 1, -30) SideMenu.Position = UDim2.new(0, 0, 0, 30) SideMenu.BackgroundColor3 = Color3.fromRGB(35, 35, 35) SideMenu.Parent = MainFrame

local TabsFrame = Instance.new("Frame") TabsFrame.Size = UDim2.new(1, -120, 1, -30) TabsFrame.Position = UDim2.new(0, 120, 0, 30) TabsFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40) TabsFrame.Parent = MainFrame

local tabs = {"MAIN", "PLAYER", "VISUALS", "FLY", "TROLLS"} local tabFrames, tabButtons, activeTab = {}, {}, nil

for i, tab in ipairs(tabs) do local btn = Instance.new("TextButton") btn.Size = UDim2.new(1, 0, 0, 35) btn.Position = UDim2.new(0, 0, 0, (i - 1) * 35) btn.Text = tab btn.TextColor3 = Color3.new(1, 1, 1) btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50) btn.Font = Enum.Font.SourceSans btn.TextSize = 16 btn.Parent = SideMenu tabButtons[tab] = btn

local frame = Instance.new("Frame")
frame.Size = UDim2.new(1, 0, 1, 0)
frame.BackgroundTransparency = 1
frame.Visible = false
frame.Parent = TabsFrame
tabFrames[tab] = frame

btn.MouseButton1Click:Connect(function()
    if activeTab then tabFrames[activeTab].Visible = false end
    activeTab = tab
    tabFrames[activeTab].Visible = true
end)

end

-- Por padrão, mostrar aba MAIN if tabFrames["MAIN"] then activeTab = "MAIN" tabFrames["MAIN"].Visible = true end

-- ESP simples: linha e quadrado ao redor dos jogadores local espEnabled = false local RunService = game:GetService("RunService") local function createESP() for _,v in pairs(game:GetService("Players"):GetPlayers()) do if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then local billboard = Instance.new("BillboardGui") billboard.Name = "JambaESP" billboard.Adornee = v.Character.HumanoidRootPart billboard.Size = UDim2.new(4,0,5,0) billboard.AlwaysOnTop = true billboard.Parent = v.Character

local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1,0,1,0)
        frame.BackgroundTransparency = 1
        frame.BorderSizePixel = 2
        frame.BorderColor3 = Color3.fromRGB(255,0,0)
        frame.Parent = billboard
    end
end

end

local function clearESP() for _,v in pairs(game:GetService("Players"):GetPlayers()) do if v.Character and v.Character:FindFirstChild("JambaESP") then v.Character.JambaESP:Destroy() end end end

RunService.RenderStepped:Connect(function() if espEnabled then clearESP() createESP() end end)

local toggleESP = Instance.new("TextButton") toggleESP.Text = "Toggle ESP" toggleESP.Size = UDim2.new(0, 120, 0, 40) toggleESP.Position = UDim2.new(0, 20, 0, 20) toggleESP.BackgroundColor3 = Color3.fromRGB(60, 60, 60) toggleESP.TextColor3 = Color3.new(1, 1, 1) toggleESP.Font = Enum.Font.SourceSans

toggleESP.TextSize = 18 toggleESP.Parent = tabFrames["VISUALS"] toggleESP.MouseButton1Click:Connect(function() espEnabled = not espEnabled end)

