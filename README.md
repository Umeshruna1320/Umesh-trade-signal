# Umesh-trade-signal

import React, { useState, useEffect } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button";

// Dummy price data for testing const dummyData = [ { open: 100, close: 95, high: 102, low: 94 }, { open: 95, close: 105, high: 106, low: 93 }, { open: 105, close: 103, high: 108, low: 102 }, { open: 103, close: 110, high: 111, low: 101 }, ];

// Function to calculate RSI function calculateRSI(data, period = 14) { if (data.length < period + 1) return null;

let gains = 0, losses = 0; for (let i = 1; i <= period; i++) { let diff = data[i].close - data[i - 1].close; if (diff > 0) gains += diff; else losses -= diff; }

const avgGain = gains / period; const avgLoss = losses / period; if (avgLoss === 0) return 100;

const rs = avgGain / avgLoss; return 100 - 100 / (1 + rs); }

// Function to detect bullish engulfing function isBullishEngulfing(prev, curr) { return prev.close < prev.open && curr.close > curr.open && curr.open < prev.close && curr.close > prev.open; }

// Function to detect bearish engulfing function isBearishEngulfing(prev, curr) { return prev.close > prev.open && curr.close < curr.open && curr.open > prev.close && curr.close < prev.open; }

export default function UmeshTradeSignal() { const [signal, setSignal] = useState("WAIT"); const [rsi, setRSI] = useState(null);

useEffect(() => { const rsiValue = calculateRSI(dummyData); setRSI(rsiValue);

const prevCandle = dummyData[dummyData.length - 2];
const currCandle = dummyData[dummyData.length - 1];

if (rsiValue !== null) {
  if (rsiValue < 30 && isBullishEngulfing(prevCandle, currCandle)) {
    setSignal("BUY");
  } else if (rsiValue > 70 && isBearishEngulfing(prevCandle, currCandle)) {
    setSignal("SELL");
  } else {
    setSignal("WAIT");
  }
}

}, []);

return ( <div className="flex justify-center items-center min-h-screen bg-gray-100"> <Card className="w-full max-w-md text-center"> <CardContent> <h1 className="text-2xl font-bold mb-4">Umesh Trade Signal</h1> <p className="text-lg">RSI: {rsi ? rsi.toFixed(2) : "Calculating..."}</p> <p className={text-xl font-bold mt-4 ${signal === "BUY" ? "text-green-600" : signal === "SELL" ? "text-red-600" : "text-gray-600"}}> Signal: {signal} </p> </CardContent> </Card> </div> ); }

