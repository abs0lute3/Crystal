## API Reference

Paste this whole block to an LLM or read it if you plan to use this manually along with "using this API, build me a
menu that does X" to get a working script, or use it as a reference while
building manually.

```lua
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
        local Window = Crystal:CreateWindow({
            Name = "My Menu",
            ToggleKey = Enum.KeyCode.RightControl,
            MinSize = Vector2.new(500, 320),
        })

        local MainTab = Window:CreateTab("Main")
        local SettingsTab = Window:CreateTab("Settings")

        -- ---- Display components ----
        MainTab:CreateSection("Overview")
        MainTab:CreateLabel("A simple label row.")
        MainTab:CreateParagraph("Information", "Press RightControl to show or hide this UI.")
        MainTab:CreateDivider()

        -- ---- Input components (each takes an optional last `flag` arg) ----
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

        -- ---- Tooltips ----
        Crystal:AttachTooltip(MainTab.Frame, "Tooltips can be attached to any GUI instance.")

        -- ---- Notifications ----
        Crystal:Notify({
            Title = "Welcome",
            Content = "The menu has loaded.",
            Type = "Info",       -- "Success" | "Warning" | "Error" | "Info"
            Duration = 4,        -- seconds, 0 = sticky until closed
        })

        -- ---- Watermark (optional FPS/title overlay) ----
        Crystal:CreateWatermark({ Title = "My Menu", ShowFPS = true })

        -- ---- Theming ----
        SettingsTab:CreateSection("Appearance")
        SettingsTab:CreateDropdown("Theme", {"Default", "Midnight", "Ocean", "Light"}, function(theme)
            Crystal:SetTheme(theme)
        end)

        -- Register your own theme:
        -- Crystal:SetCustomTheme({ Accent = Color3.fromRGB(255, 90, 90) })

        -- ---- Config persistence (flag-based) ----
        SettingsTab:CreateSection("Configuration")
        SettingsTab:CreateButton("Save Config", function()
            Crystal:SaveConfig("default")
        end)
        SettingsTab:CreateButton("Load Config", function()
            Crystal:LoadConfig("default")
        end)

    end
})
```
