import { useState } from "react";

export default function Home() {
  const [product, setProduct] = useState("");
  const [summary, setSummary] = useState("");
  const [loading, setLoading] = useState(false);

  const handleAnalyze = async () => {
    setLoading(true);
    setSummary("⏳ Analyzing reviews...");

    const res = await fetch("/api/analyze", {
      method: "POST",
      body: JSON.stringify({ query: product }),
      headers: { "Content-Type": "application/json" },
    });

    const data = await res.json();
    setSummary(data.summary || "No summary available.");
    setLoading(false);
  };

  return (
    <main className="min-h-screen p-6 bg-gray-100 font-sans">
      <div className="max-w-3xl mx-auto bg-white rounded-2xl shadow p-6 space-y-4">
        <h1 className="text-2xl font-bold text-gray-800">Market Research AI</h1>
        <p className="text-gray-600">
          Type a product or brand name to analyze customer reviews and generate insights.
        </p>
        <div className="flex gap-2">
          <input
            type="text"
            className="flex-1 border border-gray-300 rounded px-3 py-2"
            placeholder="e.g. Zapier"
            value={product}
            onChange={(e) => setProduct(e.target.value)}
          />
          <button
            onClick={handleAnalyze}
            className="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700"
            disabled={loading}
          >
            {loading ? "Analyzing..." : "Analyze"}
          </button>
        </div>

        <div className="mt-4">
          <h2 className="font-semibold text-gray-700">GPT-4 Summary:</h2>
          <pre className="whitespace-pre-wrap bg-gray-50 p-4 mt-2 rounded text-sm text-gray-800">
            {summary || "No data yet."}
          </pre>
        </div>
      </div>
    </main>
  );
}
