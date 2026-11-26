import yfinance as yf

ticker = input("Enter stock ticker: ").upper()
stock = yf.Ticker(ticker)
info = stock.info

name    = info.get("longName", "N/A")
sector  = info.get("sector", "N/A")
country = info.get("country", "N/A")
price   = info.get("currentPrice", info.get("regularMarketPrice", "N/A"))
pe      = info.get("trailingPE", "N/A")
revenue = info.get("totalRevenue", "N/A")
margin  = info.get("profitMargins", "N/A")
growth  = info.get("revenueGrowth", "N/A")
debt    = info.get("debtToEquity", "N/A")
summary = info.get("longBusinessSummary", "N/A")

margin_num = margin * 100 if margin != "N/A" else "N/A"
growth_num = growth * 100 if growth != "N/A" else "N/A"

margin_text  = f"{round(margin_num, 1)}%" if margin != "N/A" else "N/A"
growth_text  = f"{round(growth_num, 1)}%" if growth != "N/A" else "N/A"
revenue_text = f"${revenue:,}" if revenue != "N/A" else "N/A"

print("")
print("=" * 45)
print(f"  {name} ({ticker})")
print(f"  {sector} | {country}")
print("=" * 45)
print("")
print("PRICE & VALUATION")
print(f"  Current Price : ${price}")
print(f"  P/E Ratio     : {pe}")
print("")
print("FINANCIALS")
print(f"  Revenue       : {revenue_text}")
print(f"  Profit Margin : {margin_text}")
print(f"  Revenue Growth: {growth_text}")
print(f"  Debt/Equity   : {debt}")
print("")
print("ABOUT")
print(f"  {summary[:300]}...")
print("=" * 45)

return_pct = 0

if price != "N/A":
    print("\n--- YOUR POSITION ---")
    bought_at = float(input("What price did you buy at? $"))
    shares_owned = int(input("How many shares do you own? "))

    cost = bought_at * shares_owned
    current_value = price * shares_owned
    profit_loss = current_value - cost
    return_pct = ((price - bought_at) / bought_at) * 100

    print("")
    print("=" * 45)
    print("  YOUR POSITION")
    print("=" * 45)
    print(f"  Bought at       : ${bought_at}")
    print(f"  Current price   : ${price}")
    print(f"  Shares owned    : {shares_owned}")
    print(f"  Amount invested : ${cost:,.2f}")
    print(f"  Current value   : ${current_value:,.2f}")
    if profit_loss >= 0:
        print(f"  Profit          : +${profit_loss:,.2f} (+{round(return_pct, 2)}%)")
    else:
        print(f"  Loss            : -${abs(profit_loss):,.2f} ({round(return_pct, 2)}%)")
else:
    print("\nNo live price found, skipping your position.")

score = 0
flags = []

if pe != "N/A":
    if pe < 15:
        score += 2
        flags.append("✅ P/E is low, stock looks cheap")
    elif pe < 25:
        score += 1
        flags.append("✅ P/E is reasonable")
    elif pe > 40:
        score -= 2
        flags.append("⚠️ P/E is very high, could be overvalued")
else:
    flags.append("ℹ️ No P/E (company may not be profitable)")

if growth != "N/A":
    if growth_num > 10:
        score += 2
        flags.append("✅ Strong revenue growth")
    elif growth_num > 0:
        score += 1
        flags.append("✅ Revenue is growing")
    else:
        score -= 2
        flags.append("🔴 Revenue is shrinking, red flag")

if margin != "N/A":
    if margin_num > 20:
        score += 2
        flags.append("✅ Strong profit margins")
    elif margin_num > 10:
        score += 1
        flags.append("✅ Decent profit margins")
    else:
        score -= 1
        flags.append("⚠️ Thin profit margins")

if debt != "N/A":
    if debt > 200:
        score -= 2
        flags.append("🔴 Very high debt, risky")
    elif debt > 100:
        score -= 1
        flags.append("⚠️ Moderate debt, watch it")
    else:
        flags.append("✅ Debt levels look healthy")

if return_pct > 20:
    flags.append(f"💰 You're up {round(return_pct, 1)}%, consider taking some profit")
elif return_pct < -20:
    flags.append(f"🔴 You're down {round(return_pct, 1)}%, reassess your position")

print("")
print("=" * 45)
print("  ANALYST VERDICT")
print("=" * 45)

for flag in flags:
    print(f"  {flag}")

print("")
if score >= 4:
    print("  🟢 VERDICT: BUY / ADD TO POSITION")
elif score >= 1:
    print("  🟡 VERDICT: HOLD")
else:
    print("  🔴 VERDICT: SELL / AVOID")

print(f"  Fundamental Score: {score} (max 6)")
print("=" * 45)
