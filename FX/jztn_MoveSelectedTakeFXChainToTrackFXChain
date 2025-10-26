-- Script: Move selected take FX chain to track FX chain, remove from take, and show confirmation
function main()
  -- Get the selected media item
  local item = reaper.GetSelectedMediaItem(0, 0)
  if item then
    -- Get the active take
    local take = reaper.GetActiveTake(item)
    if take then
      -- Get the track of the item
      local track = reaper.GetMediaItem_Track(item)
      
      -- Count FX on the take
      local fx_count = reaper.TakeFX_GetCount(take)
      if fx_count > 0 then
        -- Table to store FX names
        local fx_names = {}
        
        -- Loop through each FX in the take
        for i = 0, fx_count - 1 do
          -- Get FX name
          local _, fx_name = reaper.TakeFX_GetFXName(take, 0, "")
          table.insert(fx_names, fx_name)
          
          -- Copy FX from take to track
          reaper.TakeFX_CopyToTrack(take, 0, track, reaper.TrackFX_GetCount(track), false)
        end
        
        -- Remove all FX from the take
        while reaper.TakeFX_GetCount(take) > 0 do
          reaper.TakeFX_Delete(take, 0)
        end
        
        -- Show confirmation message with FX names 
        local msg = string.format("Moved %d FX to track:\n%s", fx_count, table.concat(fx_names, "\n"))
        reaper.ShowConsoleMsg(msg)
        
        -- Auto-close message after 3 seconds
        local start_time = reaper.time_precise()
        local function clear_msg()
          if reaper.time_precise() - start_time >= 3 then
            reaper.ShowConsoleMsg("")
          else
            reaper.defer(clear_msg)
          end
        end
        reaper.defer(clear_msg)
      else
        reaper.ShowMessageBox("Error: No FX found on the selected take.", "No Take FX", 0)
      end
    else
      reaper.ShowMessageBox("Error: No active take selected.", "No Active Take", 0)
    end
  else
    reaper.ShowMessageBox("Error: No media item selected.", "No Media Item", 0)
  end
end

-- Run the script
main()
