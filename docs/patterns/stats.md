# Probability & Statistics

### Statistics

## Moving Average
A moving average is a way to smooth out noisy data by averaging over a sliding window of points. We lose the first and last few points, but the result is a smoother version of the original signal. Output = N - W + 1. So the first W - 1 data points don’t get included in the result (no "look-behind"). A moving average smooths the data by averaging out short-term fluctuations. It dampens highs and lows, suppressing noise.

Imagine you have this list of episode rewards:

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

If you choose a window size of 3, here’s what happens:

Take the average of the first 3 numbers:
(1 + 2 + 3) / 3 = 2

Slide the window one step and average the next 3:
(2 + 3 + 4) / 3 = 3

Keep doing this:
(3 + 4 + 5) / 3 = 4
(4 + 5 + 6) / 3 = 5
...

[2, 3, 4, 5, 6, 7, 8, 9]

Why Use It?
In reinforcement learning, the reward per episode can bounce all over the place due to randomness. A moving average helps you see the underlying trend by smoothing out that variance.

np.convolve(data, np.ones(window_size) / window_size, mode='valid')
