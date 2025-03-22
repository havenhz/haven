
local AimingChecks = Aiming.Checks
local AimingSelected = Aiming.Selected
local AimLockSettings = Aiming.AimLock


local Workspace = game:GetService("Workspace")


local CurrentCamera = Workspace.CurrentCamera

// CHANGE YOUR SETTINGS HERE
local DaHoodSettings = {
    Prediction = 0.165,

    SilentAim = true,

    AimLock = AimLockSettings,
    BeizerLock = {
        Smoothness = 0.05,
        CurvePoints = {
            Vector2.new(0.83, 0),
            Vector2.new(0.17, 1)
        }
    }
}
getgenv().DaHoodSettings = DaHoodSettings


function DaHoodSettings.ApplyPredictionFormula(SelectedPart, Velocity)
    return SelectedPart.CFrame + (Velocity * DaHoodSettings.Prediction)
end


local __index
__index = hookmetamethod(game, "__index", function(t, k)

    if (t:IsA("Mouse") and (k == "Hit" or k == "Target") and AimingChecks.IsAvailable() and DaHoodSettings.SilentAim) then

        local SelectedPart = AimingSelected.Part
        local Hit = DaHoodSettings.ApplyPredictionFormula(SelectedPart, AimingSelected.Velocity * Vector3.new(1, 0.1, 1))


        return (k == "Hit" and Hit or SelectedPart)
    end


    return __index(t, k)
end)


function AimLockSettings.AimLockPosition(CameraMode)

    local Position
    local BeizerData = {}


    local Hit = DaHoodSettings.ApplyPredictionFormula(AimingSelected.Part)
    local HitPosition = Hit.Position


    if (CameraMode) then
        Position = HitPosition
    else

        local Vector, _ = CurrentCamera:WorldToViewportPoint(HitPosition)
        local Vector2D = Vector2.new(Vector.X, Vector.Y)


        local BeizerLock = DaHoodSettings.BeizerLock


        Position = Vector2D
        BeizerData = {
            Smoothness = BeizerLock.Smoothness,
            CurvePoints = BeizerLock.CurvePoints
        }
    end


    return Position, BeizerData
end


return DaHoodSettings
