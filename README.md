# Black-Scholes Option Pricing Model

A C++ implementation of the Black-Scholes model for pricing European options and visualizing various option strategies.

## Paper

- [My paper on this project](analyzing-option-and-underlying-positions.pdf)

## What it does

- Prices call and put options using the Black-Scholes formula
- Calculates Greeks (Delta, Gamma, Theta, Vega, Rho) for individual options and entire positions
- Generates payoff diagrams at expiration
- Shows how P&L changes with volatility
- Visualizes time decay across different dates
- Includes a 3D surface plot of volatility vs price vs payoff

## Supported Strategies

The model comes with preset data for common option strategies:
- Long/Short Call
- Long/Short Put
- Butterfly Spread
- Straddle
- Strangle
- Call/Put Backspread
- Ratio Vertical Spread
- Calendar (Time) Spread

You can also input your own custom positions.

## Requirements

- C++ compiler (g++ recommended)
- GnuPlot installed on your system

## Setup

1. Install GnuPlot from https://gnuplot.info
2. Open `Cpp/Model.cpp` and update the path on line 25 to match your GnuPlot installation:
   ```cpp
   string gnu_path = "C:\\Program Files\\gnuplot\\bin\\gnuplot.exe";
   ```
3. Compile:
   ```
   g++ -o Model.exe Model.cpp -std=c++17
   ```
4. Run:
   ```
   Model.exe
   ```

## Usage

When you run the program, it will ask if you want to use demo data or enter your own. The demo mode lets you pick from the preset strategies to see how they behave.

If you choose to enter your own data, you'll be prompted for:
- Number of options in your position
- Risk-free rate
- Current stock price
- For each option: type (call/put), quantity, premium, strike, days to expiry, volatility

## Formulas

### Black-Scholes Price

Call: `C = S * N(d1) - K * e^(-rT) * N(d2)`

Put: `P = K * e^(-rT) * N(-d2) - S * N(-d1)`

Where:
```
d1 = [ln(S/K) + (r + σ²/2) * T] / (σ * √T)
d2 = d1 - σ * √T
```

- `S` = Current stock price
- `K` = Strike price
- `r` = Risk-free interest rate
- `T` = Time to maturity (in years)
- `σ` = Annualized volatility
- `N(x)` = Standard normal cumulative distribution function

### Greeks

**Delta**
- Call: `N(d1)`
- Put: `N(d1) - 1`

**Gamma** 
- (same for both): `n(d1) / (S * σ * √T)`

**Theta**
- Call: `-(S * σ * n(d1)) / (2√T) - r * K * e^(-rT) * N(d2)`
- Put: `-(S * σ * n(d1)) / (2√T) + r * K * e^(-rT) * N(-d2)`

**Vega** (same for both): `S * √T * n(d1)`

**Rho**
- Call: `K * T * e^(-rT) * N(d2)`
- Put: `-K * T * e^(-rT) * N(-d2)`

Where `n(x)` is the standard normal PDF: `n(x) = (1/√2π) * e^(-x²/2)`

### Intrinsic Value

Call: `max(0, S - K)`

Put: `max(0, K - S)`

## Notes

- Built on Windows, uses `windows.h` for getting the current directory
- Charts open in separate GnuPlot windows with `-persist` flag so they stay open
- The theoretical vs implied premium comparison shows mispricing opportunities

## Author

Matas Urbonavicius
