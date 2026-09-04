## API Reference

Paste this whole block to an LLM or read it if you plan to use this manually along with "using this API, build me a
menu that does X" to get a working script, or use it as a reference while
building manually.

```lua
-- ============================================================
-- CRYSTAL UI v3.0 - Full API Reference / LLM Prompt
-- ============================================================

local Crystal = loadstring(game:HttpGet("https://raw.githubusercontent.com/abs0lute3/Crystal/refs/heads/main/CrystalUI.lua"))()

-- ---- Optional key system (gates everything below OnSuccess) ----
Crystal:CreateKeySystem({
    Keys = {"1234", "5678"},        -- one or more accepted keys
    Title = "Crystal Verification",
    Subtitle = "Enter a valid key to continue",
    Note = "Keys are case sensitive.",
    GetKeyURL = "https://example.com/getkey",
    SaveKey = true,                 -- remembers a valid key locally
    MaxAttempts = 0,                -- 0 = unlimited
    OnSuccess = function()

        -- ---- Window ----
        -- SettingsTab = true (default): every window auto-gets a Settings
        -- tab with theme changer, toggle-key picker, config save/load,
        -- "Copy API Reference" button and unload.
        local Window = Crystal:CreateWindow({
            Name = "My Menu",
            Subtitle = "v1.0",
            ToggleKey = Enum.KeyCode.RightControl,
            Size = Vector2.new(640, 460),  -- optional initial size
            MinSize = Vector2.new(500, 320),
            ShowToggleButton = true,       -- floating pill when hidden
        })

        -- Tabs take an optional Lucide icon name ("home", "settings",
        -- "sliders-horizontal", "palette", "key", ...). Aliases like
        -- "house"/"gear" also work. Unknown strings render as text.
        local MainTab = Window:CreateTab("Main", "home")
        local SettingsTab = Window:CreateTab("Settings", "settings")

        -- Window controls
        -- Window:Minimize() Window:Restore() Window:ToggleMinimize()
        -- Window:Toggle() Window:SetVisible(true/false)
        -- Window:SetTitle("New Name", "optional subtitle")
        -- Window:ShowLoading("Working...") Window:HideLoading()
        -- Window:Destroy()

        -- ---- Display components ----
        MainTab:CreateSection("Overview")
        MainTab:CreateLabel("A simple label row.")
        MainTab:CreateParagraph("Information", "Press RightControl to show or hide this UI.")
        MainTab:CreateImage({ Title = "Banner", Image = "rbxassetid://0", Height = 100 })
        MainTab:CreateDivider()

        -- ---- Input components (each takes an optional trailing `flag` string) ----
        MainTab:CreateButton("Run Action", function()
            Crystal:Notify({ Title = "Action", Content = "Button pressed.", Type = "Success", Duration = 3 })
        end)

        MainTab:CreateToggle("Feature Toggle", false, function(state)
            print("Feature toggle:", state)
        end, "featureToggle")

        MainTab:CreateSlider("Adjustment Value", 0, 100, 50, function(value)
            print("Value set to:", value)
        end, "adjustmentValue")

        MainTab:CreateInput("Custom Text", "Type here...", function(text, enterPressed)
            print("Input:", text)
        end, "customText")

        MainTab:CreateDropdown("Mode", {"Option A", "Option B", "Option C"}, function(selected)
            print("Mode selected:", selected)
        end, "modeSelect")

        MainTab:CreateMultiDropdown("Categories", {"Alpha", "Beta", "Gamma"}, function(selectedList)
            print("Selected:", table.concat(selectedList, ", "))
        end, "categories")

        MainTab:CreateKeybind("Action Keybind", Enum.KeyCode.E, function(key)
            print("New keybind:", key.Name)
        end, "actionKey")

        MainTab:CreateColorPicker("Accent Color", Color3.fromRGB(99, 102, 241), function(color)
            print("Color:", color)
        end, "accentColor")

        -- Component handles: most return a table with Set/Get, e.g.
        -- local t = MainTab:CreateToggle("X", false, function() end, "xFlag")
        -- t:Set(true) print(t:Get())
        -- Dropdowns also have :Refresh({"New", "Options"}).

        -- ---- Tooltips (Lucide-free text hints) ----
        Crystal:AttachTooltip(MainTab.Frame, "Tooltips can be attached to any GUI instance.")

        -- ---- Notifications ----
        Crystal:Notify({
            Title = "Welcome",
            Content = "The menu has loaded.",
            Type = "Info",       -- "Success" | "Warning" | "Error" | "Info"
            Duration = 4,        -- seconds, 0 = sticky until closed
        })

        -- ---- Modal prompt ----
        Crystal:Prompt({
            Title = "Are you sure?",
            Content = "This cannot be undone.",
            Buttons = { "Confirm", "Cancel" },
            Callback = function(choice) print("Picked:", choice) end,
        })

        -- ---- Watermark (optional FPS/ping overlay, draggable) ----
        Crystal:CreateWatermark({ Title = "My Menu", ShowFPS = true, ShowPing = false })

        -- ---- Theming (10 built-ins, live accent supported) ----
        -- "Default" | "Midnight" | "Ocean" | "Light" | "Abyss" |
        -- "Amber" | "Amethyst" | "Bloom" | "Emerald" | "Crimson"
        SettingsTab:CreateSection("Appearance")
        SettingsTab:CreateDropdown("Theme", {"Default", "Midnight", "Ocean", "Light"}, function(theme)
            Crystal:SetTheme(theme)
        end)

        -- Recolor just the accent, keep the rest of the active theme:
        Crystal:SetCustomTheme({ Accent = Color3.fromRGB(255, 90, 90) })
        -- Register a full extra theme: Crystal:AddTheme("Mine", { Accent = ... })

        -- ---- Config persistence (flag-based, saves theme too) ----
        SettingsTab:CreateSection("Configuration")
        SettingsTab:CreateButton("Save Config", function()
            Crystal:SaveConfig("default")
        end)
        SettingsTab:CreateButton("Load Config", function()
            Crystal:LoadConfig("default")
        end)
        -- Crystal:ListConfigs() Crystal:DeleteConfig("default")
        -- Crystal:GetFlag("modeSelect") Crystal:SetFlag("modeSelect", "Option B")

        -- ---- Cleanup ----
        -- Crystal:Unload()            -- destroy every Crystal window
        -- Crystal:CopyDocumentation() -- copy THIS reference (also a button
        --                              -- in the auto Settings tab)

    end
})

```
