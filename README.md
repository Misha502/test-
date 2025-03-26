import React, { useState } from "react";
import { Button, Card, CardContent, Input, Label } from "@/components/ui";

const questions = [
  {
    section: "Čím podle mého názoru můžu přispět do týmu?",
    statements: [
      "Myslím, že můžu rychle postřehnout a využít nové příležitosti.",
      "Umím dobře pracovat s širokým spektrem lidí.",
      "Jedním z mých přirozených talentů je produkování nápadů.",
      "Moje schopnosti spočívají v tom, že dokážu z lidí dostat vše co je potřeba.",
      "Moje schopnost jít vpřed a dotáhnout věci do konce úzce souvisí s mojí osobní efektivností.",
      "Nevadí mi čelit dočasné ztrátě popularity.",
      "Obvykle vycítím, co je realistické a bude pravděpodobně fungovat."
    ],
    roles: ["Hledač zdrojů", "Týmový pracovník", "Myslitel", "Koordinátor", "Realizátor", "Formovač", "Hodnotitel"]
  },
  {
    section: "Pokud mám v týmové práci nějaký nedostatek, může to být tím, že:",
    statements: [
      "Necítím se dobře, když jsou porady špatně strukturované.",
      "Mám sklony být velmi liberální k lidem.",
      "Mám tendenci hodně mluvit, když se tým začne zabývat novými myšlenkami.",
      "Můj objektivní pohled mi komplikuje přidat se pohotově ke kolegům.",
      "Někdy mě považují za autoritativního a energického.",
      "Připadá mi složité vést z přední pozice.",
      "Jsem náchylný nechat se unést vlastními myšlenkami.",
      "Mý kolegové mají sklon vidět mě jako příliš starostlivého o detaily."
    ],
    roles: ["Dokončovatel", "Týmový pracovník", "Myslitel", "Hodnotitel", "Formovač", "Koordinátor", "Hledač zdrojů"]
  }
];

const roles = {
  "Realizátor": 0,
  "Koordinátor": 0,
  "Formovač": 0,
  "Myslitel": 0,
  "Hledač zdrojů": 0,
  "Hodnotitel": 0,
  "Týmový pracovník": 0,
  "Dokončovatel": 0
};

export default function BelbinTest() {
  const [responses, setResponses] = useState(
    questions.map((q) => q.statements.map(() => ""))
  );
  const [result, setResult] = useState(null);

  const handleInputChange = (sectionIdx, statementIdx, value) => {
    const newResponses = [...responses];
    newResponses[sectionIdx][statementIdx] = value;
    setResponses(newResponses);
  };

  const calculateResult = () => {
    let scores = { ...roles };
    responses.forEach((section, sectionIdx) => {
      section.forEach((value, statementIdx) => {
        scores[questions[sectionIdx].roles[statementIdx]] += Number(value) || 0;
      });
    });
    const topRole = Object.keys(scores).reduce((a, b) => (scores[a] > scores[b] ? a : b));
    setResult(topRole);
  };

  return (
    <div className="p-4">
      <h1 className="text-xl font-bold mb-4">Belbinův Test Týmových Rolí</h1>
      {questions.map((section, sectionIdx) => (
        <Card key={sectionIdx} className="mb-4 p-4">
          <h2 className="text-lg font-semibold">{section.section}</h2>
          <CardContent>
            {section.statements.map((statement, statementIdx) => (
              <div key={statementIdx} className="mb-2">
                <Label>{statement}</Label>
                <Input
                  type="number"
                  min="0"
                  max="10"
                  value={responses[sectionIdx][statementIdx]}
                  onChange={(e) =>
                    handleInputChange(sectionIdx, statementIdx, e.target.value)
                  }
                />
              </div>
            ))}
          </CardContent>
        </Card>
      ))}
      <Button className="mt-4" onClick={calculateResult}>Vyhodnotit</Button>
      {result && <h2 className="mt-4 text-lg font-bold">Vaše týmová role: {result}</h2>}
    </div>
  );
}
