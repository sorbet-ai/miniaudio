# Sorbet AI's miniaudio

miniaudio 0.11.25 (tag `0.11.25`, commit `9634bedb5b5a2ca38c1ee7108a9358a4e233f14d`) with a few
patches, each compiled only when its macro is defined (without the macros it is miniaudio's own
code). Search miniaudio.h for "sorbet patch".

| macro | what |
|---|---|
| `MA_WASAPI_EXCLUSIVE_CAPTURE_PREFER_S24` | an exclusive WASAPI capture stream asks for 24-bit samples in 3 bytes at the device's rate and channels first, keeping Windows' default format when refused |
| `MA_CAPTURE_PACKET_HOOK(device, frames, flags, pos, time)` | called with every input buffer's device position and time (WASAPI GetBuffer's position and QPC; Core Audio's sample and host time) |
| `MA_PLAYBACK_TIME_HOOK(device, frames, time)` | Core Audio: the host time the buffer about to be rendered is presented at |
| `MA_WASAPI_SHARED_QUEUE` | `ma_device_config.wasapi.sharedQueueFrames`: a shared WASAPI playback stream keeps at most that many frames queued (0: the whole buffer, miniaudio's behaviour); `ma_device.wasapi.sharedLowLatencyPlayback` says whether the stream runs on IAudioClient3's engine period |
| `MA_WASAPI_SHARED_PERIOD_AT_LEAST` | `ma_device_config.wasapi.sharedPeriodAtLeast`: low-latency shared mode (IAudioClient3) takes the smallest engine period `min + k * fundamental` that is at least the period asked, instead of rounding down and falling back |

Branch `sorbet-0.11.25`. Used by Sorbet AI's native audio library, pinned by commit. The patches
are carried over when miniaudio is updated.
