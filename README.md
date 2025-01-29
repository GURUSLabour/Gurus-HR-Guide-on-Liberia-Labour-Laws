import React, { useState, useEffect } from "react";
import { Input } from "@/components/ui/input";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Search, Sun, Moon, Download, Edit } from "lucide-react";
import axios from "axios";

const CMS_API_URL = "https://cms.example.com/api/data";

export default function App() {
  const [search, setSearch] = useState("");
  const [darkMode, setDarkMode] = useState(false);
  const [data, setData] = useState([]);

  useEffect(() => {
    axios.get(CMS_API_URL)
      .then((response) => setData(response.data))
      .catch((error) => console.error("Error fetching CMS data:", error));
  }, []);

  const filteredData = data.filter((item) =>
    item.title.toLowerCase().includes(search.toLowerCase())
  );

  const toggleDarkMode = () => {
    setDarkMode(!darkMode);
    document.documentElement.classList.toggle("dark");
  };

  return (
    <div className={`p-6 max-w-2xl mx-auto ${darkMode ? "bg-gray-900 text-white" : "bg-white text-black"}`}>
      <div className="flex justify-between items-center mb-4">
        <h1 className="text-2xl font-bold text-center">Decent Work Act - Liberia</h1>
        <Button variant="outline" onClick={toggleDarkMode}>
          {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />}
        </Button>
      </div>
      <div className="flex items-center gap-2 mb-4">
        <Input
          type="text"
          placeholder="Search topics..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />
        <Button variant="outline">
          <Search className="w-5 h-5" />
        </Button>
      </div>
      <div className="space-y-4">
        {filteredData.length > 0 ? (
          filteredData.map((item, index) => (
            <Card key={index} className="p-4 border border-gray-200">
              <CardContent>
                <div className="flex justify-between">
                  <h2 className="font-semibold text-lg">{item.title}</h2>
                  <Button variant="ghost">
                    <Edit className="w-5 h-5" />
                  </Button>
                </div>
                <p className="mt-2">{item.content}</p>
              </CardContent>
            </Card>
          ))
        ) : (
          <p className="text-center text-gray-500">No results found.</p>
        )}
      </div>
      <div className="mt-6 flex justify-center">
        <Button variant="outline">
          <Download className="w-5 h-5 mr-2" /> Download PDF
        </Button>
      </div>
    </div>
  );
}
