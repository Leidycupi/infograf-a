import React, { useState, useEffect } from 'react';
import { Download, MapPin, Building2, Layers, Lightbulb, BookOpen, Info, HelpCircle } from 'lucide-react';

const App = () => {
  const [imageUrl, setImageUrl] = useState(null);
  const [isGenerating, setIsGenerating] = useState(true);
  const [showHelp, setShowHelp] = useState(false);

  const apiKey = ""; // La clave se proporciona en el entorno de ejecución

  const generateMainImage = async () => {
    try {
      const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/imagen-4.0-generate-001:predict?key=${apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          instances: [{ 
            prompt: "Modern architectural photography of CaixaForum Zaragoza by Carme Pinós, two suspended geometric aluminum volumes, golden hour lighting, sharp lines, cinematic wide angle" 
          }],
          parameters: { sampleCount: 1 }
        })
      });
      const result = await response.json();
      if (result.predictions && result.predictions[0]) {
        setImageUrl(`data:image/png;base64,${result.predictions[0].bytesBase64Encoded}`);
      }
    } catch (error) {
      console.error("Error generating image:", error);
    } finally {
      setIsGenerating(false);
    }
  };

  useEffect(() => {
    generateMainImage();
  }, []);

  const handleDownload = () => {
    setShowHelp(true);
    // Damos un segundo para que el usuario lea el mensaje antes de abrir el diálogo
    setTimeout(() => {
      window.print();
      setShowHelp(false);
    }, 2000);
  };

  return (
    <div className="min-h-screen bg-slate-50 text-slate-900 font-sans p-4 md:p-8 print:p-0">
      {/* Mensaje de ayuda para descarga (Solo visible al intentar descargar) */}
      {showHelp && (
        <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/60 backdrop-blur-sm print:hidden">
          <div className="bg-white p-8 rounded-2xl shadow-2xl max-w-sm text-center border-t-4 border-amber-500 animate-in fade-in zoom-in duration-300">
            <div className="bg-amber-100 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4">
              <Download className="text-amber-600" size={32} />
            </div>
            <h3 className="text-xl font-bold mb-2">Preparando tu PDF</h3>
            <p className="text-slate-600 mb-4">
              En la ventana que aparecerá a continuación, asegúrate de seleccionar 
              <span className="font-bold text-slate-900"> "Guardar como PDF" </span> 
              en la opción de <span className="italic">Destino</span> de la impresora.
            </p>
            <div className="flex items-center justify-center gap-2 text-xs text-amber-700 font-medium bg-amber-50 p-2 rounded">
              <HelpCircle size={14} /> Iniciando diálogo de impresión...
            </div>
          </div>
        </div>
      )}

      {/* Container Principal */}
      <div id="infographic" className="max-w-5xl mx-auto bg-white shadow-2xl rounded-xl overflow-hidden print:shadow-none print:rounded-none">
        
        {/* Header & Bio */}
        <header className="relative bg-black text-white p-8 md:p-12 overflow-hidden">
          <div className="absolute top-0 right-0 w-1/2 h-full bg-slate-800 opacity-20 -skew-x-12 translate-x-1/4"></div>
          <div className="relative z-10">
            <h1 className="text-5xl md:text-7xl font-bold tracking-tighter mb-4">CARME PINÓS</h1>
            <p className="text-xl md:text-2xl font-light text-slate-300 max-w-2xl border-l-4 border-amber-500 pl-6">
              "La arquitectura es, sobre todo, generosidad. Es crear un lugar donde la gente se sienta libre."
            </p>
          </div>
        </header>

        <section className="p-8 md:p-12 border-b border-slate-100">
          <div className="flex flex-col md:flex-row gap-8 items-start">
            <div className="flex-1">
              <h2 className="text-2xl font-bold mb-4 flex items-center gap-2 text-amber-600">
                <Info size={24} /> BIOGRAFÍA
              </h2>
              <div className="space-y-4 text-slate-700 leading-relaxed">
                <p>
                  Nacida en Barcelona en 1954, Carme Pinós es una de las arquitectas españolas con mayor proyección internacional. Tras alcanzar el reconocimiento mundial junto a Enric Miralles en los años 80, fundó su propio estudio en 1991.
                </p>
                <p>
                  Su trabajo se caracteriza por un compromiso profundo con el entorno, la estructura vista y la creación de espacios públicos dinámicos. Ha sido galardonada con el Premio Nacional de Arquitectura de España (2021) y el Berkeley-Rupp Prize, entre otros.
                </p>
              </div>
            </div>
            <div className="w-full md:w-1/3 bg-slate-100 p-6 rounded-lg italic text-slate-600 text-sm">
              "No busco la forma por la forma, busco la respuesta a un lugar y a un programa." 
              <br/><br/>
              — Carme Pinós en su estudio de Gràcia.
            </div>
          </div>
        </section>

        {/* Proyecto Destacado */}
        <section className="p-8 md:p-12 bg-slate-900 text-white">
          <div className="mb-8">
            <span className="bg-amber-500 text-black px-3 py-1 text-xs font-bold uppercase tracking-widest rounded">Proyecto Destacado</span>
            <h2 className="text-4xl font-bold mt-2">CAIXAFORUM ZARAGOZA</h2>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-12">
            <div className="space-y-6">
              {isGenerating ? (
                <div className="w-full aspect-video bg-slate-800 animate-pulse rounded-lg flex items-center justify-center">
                  <span className="text-slate-500">Generando visualización...</span>
                </div>
              ) : (
                <img 
                  src={imageUrl} 
                  alt="CaixaForum Zaragoza" 
                  className="w-full rounded-lg shadow-xl hover:scale-[1.02] transition-transform duration-500" 
                />
              )}
              
              <div className="bg-white/5 p-6 rounded-lg border border-white/10">
                <h3 className="text-amber-400 font-bold mb-2 flex items-center gap-2">
                  <Lightbulb size={18} /> ¿Por qué llama la atención?
                </h3>
                <p className="text-sm text-slate-300">
                  Su radicalidad estructural. El edificio parece "levitar" sobre la ciudad, liberando la planta baja para crear una plaza pública. Es un desafío a la gravedad que invita a mirar hacia arriba y participar en la vida urbana.
                </p>
              </div>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 gap-6">
              <div className="p-4 border-l-2 border-slate-700">
                <div className="flex items-center gap-2 text-amber-500 mb-2 font-semibold">
                  <MapPin size={18} /> Ubicación
                </div>
                <p className="text-sm text-slate-300">Avenida de Anselmo Clavé, 4, 50004 Zaragoza, España.</p>
              </div>

              <div className="p-4 border-l-2 border-slate-700">
                <div className="flex items-center gap-2 text-amber-500 mb-2 font-semibold">
                  <Building2 size={18} /> Uso
                </div>
                <p className="text-sm text-slate-300">Centro cultural: salas de exposiciones, auditorio, cafetería y espacios educativos.</p>
              </div>

              <div className="p-4 border-l-2 border-slate-700">
                <div className="flex items-center gap-2 text-amber-500 mb-2 font-semibold">
                  <Layers size={18} /> Materiales
                </div>
                <p className="text-sm text-slate-300">Aluminio perforado en fachadas, hormigón armado, estructuras de acero y vidrio de gran formato.</p>
              </div>

              <div className="p-4 border-l-2 border-slate-700">
                <div className="flex items-center gap-2 text-amber-500 mb-2 font-semibold">
                  <Lightbulb size={18} /> Inspiración
                </div>
                <p className="text-sm text-slate-300">La idea de "un bosque de columnas" que sostiene dos grandes cajas. La forma fragmentada busca dialogar con la escala de la ciudad.</p>
              </div>
            </div>
          </div>
        </section>

        {/* Sección Técnica: Planos, Fachadas y Plantas */}
        <section className="p-8 md:p-12">
          <h2 className="text-2xl font-bold mb-8 border-b pb-2 text-slate-800 uppercase tracking-tight">Análisis Técnico</h2>
          
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            {/* Planta */}
            <div className="space-y-4">
              <h3 className="font-bold text-slate-600 uppercase text-xs tracking-widest">Planta de Distribución</h3>
              <div className="bg-slate-100 aspect-square rounded border-2 border-dashed border-slate-300 p-4 relative flex flex-col items-center justify-center">
                <svg viewBox="0 0 100 100" className="w-full h-full opacity-60">
                  <rect x="10" y="20" width="30" height="60" fill="none" stroke="currentColor" strokeWidth="1" />
                  <rect x="40" y="10" width="50" height="40" fill="none" stroke="currentColor" strokeWidth="1" />
                  <circle cx="50" cy="50" r="5" fill="currentColor" />
                  <line x1="0" y1="50" x2="100" y2="50" stroke="currentColor" strokeWidth="0.5" strokeDasharray="2" />
                </svg>
                <p className="text-[10px] text-slate-400 absolute bottom-2 font-mono">ESQUEMA PLANTA NIVEL +1</p>
              </div>
              <p className="text-xs text-slate-500 italic">Las plantas se dividen en dos grandes niveles suspendidos, minimizando el impacto en el suelo.</p>
            </div>

            {/* Fachada */}
            <div className="space-y-4">
              <h3 className="font-bold text-slate-600 uppercase text-xs tracking-widest">Fachada / Envolvente</h3>
              <div className="bg-slate-100 aspect-square rounded border-2 border-dashed border-slate-300 p-4 relative flex flex-col items-center justify-center">
                <div className="grid grid-cols-6 gap-2 w-full h-full opacity-30">
                  {Array.from({length: 36}).map((_, i) => (
                    <div key={i} className="bg-slate-800 rounded-full w-2 h-2"></div>
                  ))}
                </div>
                <div className="absolute inset-0 flex items-center justify-center">
                   <svg viewBox="0 0 100 100" className="w-4/5 h-4/5">
                      <path d="M10,80 L40,80 L40,20 L90,20 L90,50 L10,50 Z" fill="none" stroke="black" strokeWidth="1" />
                   </svg>
                </div>
                <p className="text-[10px] text-slate-400 absolute bottom-2 font-mono">DETALLE PANELES PERFORADOS</p>
              </div>
              <p className="text-xs text-slate-500 italic">La piel de aluminio permite el paso de luz filtrada, creando un juego de sombras interior.</p>
            </div>

            {/* Planos (Sección) */}
            <div className="space-y-4">
              <h3 className="font-bold text-slate-600 uppercase text-xs tracking-widest">Sección Transversal</h3>
              <div className="bg-slate-100 aspect-square rounded border-2 border-dashed border-slate-300 p-4 relative flex flex-col items-center justify-center">
                <svg viewBox="0 0 100 100" className="w-full h-full opacity-60">
                  <path d="M10,90 L90,90" stroke="black" strokeWidth="2" />
                  <path d="M30,90 L30,40 M70,90 L70,40" stroke="black" strokeWidth="1.5" />
                  <rect x="15" y="20" width="70" height="20" fill="none" stroke="black" strokeWidth="1" />
                  <rect x="25" y="40" width="50" height="15" fill="none" stroke="black" strokeWidth="1" />
                </svg>
                <p className="text-[10px] text-slate-400 absolute bottom-2 font-mono">ESQUEMA ESTRUCTURAL</p>
              </div>
              <p className="text-xs text-slate-500 italic">Muestra los grandes pilares que soportan los volúmenes en voladizo.</p>
            </div>
          </div>
        </section>

        {/* Footer: Fuentes */}
        <footer className="bg-slate-50 p-8 border-t border-slate-200">
          <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-6">
            <div className="space-y-2">
              <h4 className="text-sm font-bold flex items-center gap-2 text-slate-700">
                <BookOpen size={16} /> FUENTES BIBLIOGRÁFICAS
              </h4>
              <ul className="text-xs text-slate-500 space-y-1 list-disc ml-4">
                <li>Estudio Carme Pinós - Archivo de Proyectos (caixafórum zaragoza)</li>
                <li>Revista El Croquis, N. 159: Carme Pinós 2000-2012</li>
                <li>ArchDaily - "CaixaForum Zaragoza / Estudio Carme Pinós"</li>
                <li>Premio Nacional de Arquitectura de España 2021 - MITMA</li>
              </ul>
            </div>
            
            <button 
              onClick={handleDownload}
              className="flex items-center gap-2 bg-amber-600 hover:bg-amber-700 text-white px-8 py-4 rounded-full font-bold transition-all shadow-lg hover:shadow-xl active:scale-95 print:hidden group"
            >
              <Download size={20} className="group-hover:bounce" /> Descargar PDF
            </button>
          </div>
        </footer>
      </div>

      <style>{`
        @media print {
          body { 
            background: white !important; 
            -webkit-print-color-adjust: exact !important; 
            print-color-adjust: exact !important;
          }
          .print\\:hidden { display: none !important; }
          #infographic { 
            box-shadow: none !important; 
            border: none !important; 
            width: 100% !important;
            margin: 0 !important;
          }
          header { background-color: black !important; color: white !important; }
          section.bg-slate-900 { background-color: #0f172a !important; color: white !important; }
          .bg-amber-500 { background-color: #f59e0b !important; }
        }
        @keyframes bounce {
          0%, 100% { transform: translateY(0); }
          50% { transform: translateY(-3px); }
        }
        .group-hover\\:bounce {
          animation: bounce 1s infinite;
        }
      `}</style>
    </div>
  );
};

export default App;
