# ambio
forest_storm.py
from pydub import AudioSegment
from pydub.generators import WhiteNoise
import random

# Duration (2 minutes)
duration_ms = 2 * 60 * 1000

# Wind – soft low-frequency noise
wind = WhiteNoise().to_audio_segment(duration=duration_ms, volume=-30).low_pass_filter(800)

# Rain – gentle high-frequency noise
rain = WhiteNoise().to_audio_segment(duration=duration_ms, volume=-25).high_pass_filter(3000).low_pass_filter(8000)

# Thunder – occasional low rumbles
thunder = AudioSegment.silent(duration=duration_ms)
for _ in range(5):
    start = random.randint(0, duration_ms - 5000)
    thunder_boom = WhiteNoise().to_audio_segment(duration=4000, volume=-10).low_pass_filter(200)
    thunder_boom = thunder_boom.fade_in(500).fade_out(1500)
    thunder = thunder.overlay(thunder_boom, position=start)

# Mix everything together
forest_ambience = wind.overlay(rain).overlay(thunder)

# Export as MP3
forest_ambience.export("forest_gentle_storm.mp3", format="mp3")

print("✅ Your forest storm ambience has been saved as forest_gentle_storm.mp3")
