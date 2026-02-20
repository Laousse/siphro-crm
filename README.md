[index (2).html](https://github.com/user-attachments/files/25442137/index.2.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="description" content="SIPHRO CRM — Suivi commercial CHR Méditerranée">
<meta name="robots" content="noindex, nofollow">
<title>SIPHRO CRM — Suivi Commercial</title>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Fraunces:opsz,wght@9..144,600;9..144,700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
<style>
:root{
  --bg:#f4f6fb;--bg2:#fff;--bg3:#f0f2f8;--bg4:#e8ecf4;
  --txt:#1a1d2e;--txt2:#5c6480;--txt3:#9ca3af;
  --bl:#e2e6f0;--bm:#c8cfe0;
  --blue:#2563eb;--blue-l:#dbeafe;--blue-d:#1d4ed8;
  --green:#059669;--green-l:#d1fae5;
  --orange:#d97706;--orange-l:#fef3c7;
  --red:#dc2626;--red-l:#fee2e2;
  --purple:#7c3aed;--purple-l:#ede9fe;
  --teal:#0891b2;--teal-l:#cffafe;
  --sh1:0 1px 3px rgba(0,0,0,.06);
  --sh2:0 4px 16px rgba(0,0,0,.09);
  --sh3:0 16px 48px rgba(0,0,0,.14);
  --r1:6px;--r2:10px;--r3:14px;--r4:20px;--r5:28px;
  --sidebar:268px;
}
*{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth}
body{font-family:'Plus Jakarta Sans',sans-serif;background:var(--bg);color:var(--txt);line-height:1.6;min-height:100vh;font-size:14px}

/* ── LAYOUT ── */
.app{display:flex;height:100vh;overflow:hidden}
.sidebar{width:var(--sidebar);background:linear-gradient(180deg,#1a2340 0%,#0d1526 100%);color:#fff;padding:0;position:fixed;height:100vh;overflow-y:auto;overflow-x:hidden;z-index:200;display:flex;flex-direction:column;scrollbar-width:thin;scrollbar-color:rgba(255,255,255,.2) transparent}
.sidebar::-webkit-scrollbar{width:4px}
.sidebar::-webkit-scrollbar-track{background:transparent}
.sidebar::-webkit-scrollbar-thumb{background:rgba(255,255,255,.2);border-radius:10px}
.sidebar::-webkit-scrollbar-thumb:hover{background:rgba(255,255,255,.3)}
.main{flex:1;margin-left:var(--sidebar);height:100vh;overflow:hidden;display:flex;flex-direction:column}
.view{display:none;flex:1;flex-direction:column;overflow:hidden;min-height:0}
.view.active{display:flex}
#dash-content{display:flex;flex-direction:column;flex:1;overflow:hidden;min-height:0}

/* ── SIDEBAR (compact) ── */
.sb-brand{padding:16px 16px 12px;border-bottom:1px solid rgba(255,255,255,.07)}
.sb-brand-row{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.sb-icon{width:36px;height:36px;background:linear-gradient(135deg,#3b82f6,#8b5cf6);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:16px;box-shadow:0 4px 14px rgba(59,130,246,.4)}
.sb-title{font-family:'Fraunces',serif;font-size:17px;font-weight:700;line-height:1}
.sb-sub{font-size:9.5px;color:rgba(255,255,255,.4);letter-spacing:1.2px;text-transform:uppercase}
.sb-sync{font-size:10.5px;padding:5px 8px;background:rgba(255,255,255,.07);border-radius:7px;color:rgba(255,255,255,.55);display:flex;align-items:center;gap:5px}
.sb-nav{padding:10px 10px;flex:1}
.sb-sec{margin-bottom:18px}
.sb-sec-lbl{font-size:9px;text-transform:uppercase;letter-spacing:1.8px;color:rgba(255,255,255,.28);padding:0 8px;margin-bottom:4px;font-weight:700}
.ni{display:flex;align-items:center;gap:9px;padding:8px 10px;border-radius:8px;cursor:pointer;transition:all .15s;margin-bottom:1px;color:rgba(255,255,255,.6);font-size:12.5px;font-weight:500;position:relative}
.ni:hover{background:rgba(255,255,255,.07);color:#fff}
.ni.active{background:rgba(59,130,246,.22);color:#fff;border-left:3px solid #60a5fa}
.ni-ico{font-size:14px;width:18px;text-align:center;flex-shrink:0}
.ni-lbl{flex:1}
.nbadge{margin-left:auto;font-size:9.5px;font-weight:700;padding:1px 6px;border-radius:8px;min-width:18px;text-align:center;background:rgba(239,68,68,.85);color:#fff}
.nbadge.b-blue{background:rgba(59,130,246,.75)}
.nbadge.b-green{background:rgba(5,150,105,.75)}
.nbadge.b-orange{background:rgba(217,119,6,.8)}
.nbadge.b-gray{background:rgba(100,116,139,.6)}
.dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}
.sb-footer{padding:10px 16px;border-top:1px solid rgba(255,255,255,.07)}
.sb-version{font-size:9.5px;color:rgba(255,255,255,.22);text-align:center}

/* ══════════════════════════════════════
   TOP BAR — COMPACT UNIFIED CHROME
   Remplace : header + stats bar + filters
   Hauteur totale cible : ~112px au lieu de ~230px
══════════════════════════════════════ */

/* Barre principale (ligne 1) */
.top-bar{
  background:var(--bg2);
  border-bottom:1px solid var(--bl);
  flex-shrink:0;
  z-index:100;
}

/* Ligne 1 : titre + stats chips + actions */
.tb-row1{
  display:flex;align-items:center;gap:12px;
  padding:10px 24px;
  border-bottom:1px solid var(--bl);
}
.tb-title-wrap{display:flex;flex-direction:column;min-width:0}
.tb-title{font-family:'Fraunces',serif;font-size:20px;font-weight:700;letter-spacing:-.3px;line-height:1.1;white-space:nowrap}
.tb-sub{color:var(--txt3);font-size:10.5px;margin-top:1px;white-space:nowrap}

/* Mini stats chips inline */
.tb-stats{display:flex;gap:6px;flex:1;justify-content:center;flex-wrap:nowrap;overflow:hidden}
.tb-chip{
  display:flex;align-items:center;gap:5px;
  padding:5px 11px;border-radius:20px;
  font-size:11.5px;font-weight:700;
  border:1.5px solid var(--bl);
  background:var(--bg);
  cursor:pointer;transition:all .18s;white-space:nowrap;
}
.tb-chip:hover{border-color:var(--bm);box-shadow:var(--sh1);transform:translateY(-1px)}
.tb-chip.active{background:var(--blue-l);border-color:var(--blue);color:var(--blue)}
.tb-chip.c-red.active{background:var(--red-l);border-color:var(--red);color:var(--red)}
.tb-chip.c-orange.active{background:var(--orange-l);border-color:var(--orange);color:var(--orange)}
.tb-chip.c-green.active{background:var(--green-l);border-color:var(--green);color:var(--green)}
.tb-chip-val{font-size:15px;font-weight:800;color:var(--txt)}
.tb-chip.active .tb-chip-val{color:inherit}
.tb-chip-lbl{font-size:10px;color:var(--txt3);font-weight:600;text-transform:uppercase;letter-spacing:.3px}

.tb-actions{display:flex;gap:6px;align-items:center;flex-shrink:0}
.sync-pill{font-size:10.5px;font-weight:600;padding:5px 11px;background:var(--bg3);border-radius:16px;border:1px solid var(--bl);white-space:nowrap}

/* Ligne 2 : recherche + filtres tabs + tri + vues */
.tb-row2{
  display:flex;align-items:center;gap:8px;
  padding:8px 24px;
}
.search-wrap{flex:1;min-width:200px;position:relative}
.search-wrap input{width:100%;padding:8px 14px 8px 36px;border:1.5px solid var(--bl);border-radius:20px;font-size:13px;font-family:inherit;background:var(--bg);transition:all .2s;color:var(--txt)}
.search-wrap input:focus{outline:none;border-color:var(--blue);background:#fff;box-shadow:0 0 0 3px rgba(37,99,235,.08)}
.search-wrap .si{position:absolute;left:12px;top:50%;transform:translateY(-50%);color:var(--txt3);font-size:15px}
.search-wrap .clear-btn{position:absolute;right:10px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(--txt3);font-size:16px;display:none;padding:0;line-height:1}
.search-wrap input:not(:placeholder-shown)~.clear-btn{display:block}

.ftabs{display:flex;gap:3px;background:var(--bg3);padding:3px;border-radius:10px;flex-wrap:nowrap;overflow-x:auto;scrollbar-width:none}
.ftabs::-webkit-scrollbar{display:none}
.ftab{padding:6px 11px;border-radius:7px;font-size:11.5px;font-weight:600;cursor:pointer;transition:all .15s;background:transparent;border:none;color:var(--txt2);display:flex;align-items:center;gap:5px;font-family:inherit;white-space:nowrap}
.ftab:hover{color:var(--txt);background:rgba(255,255,255,.5)}
.ftab.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.ftab .cnt{background:var(--bg4);padding:1px 6px;border-radius:6px;font-size:9.5px;font-weight:700}
.ftab.active .cnt{background:var(--blue-l);color:var(--blue)}

.sort-sel{padding:7px 10px;border:1.5px solid var(--bl);border-radius:8px;font-size:12px;font-family:inherit;background:var(--bg);cursor:pointer;color:var(--txt);outline:none;flex-shrink:0}
.sort-sel:focus{border-color:var(--blue)}
.view-tog{display:flex;gap:2px;background:var(--bg3);padding:3px;border-radius:8px;flex-shrink:0}
.vtb{width:30px;height:30px;border:none;background:transparent;border-radius:6px;cursor:pointer;font-size:14px;display:flex;align-items:center;justify-content:center;color:var(--txt2);transition:all .15s}
.vtb.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}

/* ── BUTTONS ── */
.btn{display:inline-flex;align-items:center;gap:6px;padding:8px 16px;border-radius:var(--r2);font-size:12.5px;font-weight:600;cursor:pointer;transition:all .18s;border:none;font-family:inherit;white-space:nowrap}
.btn-primary{background:var(--blue);color:#fff;box-shadow:0 2px 8px rgba(37,99,235,.3)}.btn-primary:hover{background:var(--blue-d);transform:translateY(-1px);box-shadow:0 4px 14px rgba(37,99,235,.4)}
.btn-secondary{background:var(--bg3);color:var(--txt);border:1.5px solid var(--bl)}.btn-secondary:hover{background:var(--bl)}
.btn-green{background:var(--green);color:#fff;box-shadow:0 2px 8px rgba(5,150,105,.3)}.btn-green:hover{background:#047857}
.btn-danger{background:var(--red-l);color:var(--red);border:1px solid #fecaca}.btn-danger:hover{background:var(--red);color:#fff}
.btn-sm{padding:6px 12px;font-size:12px}
.btn-xs{padding:4px 9px;font-size:11px}
.ibt{width:30px;height:30px;border:none;background:transparent;border-radius:8px;cursor:pointer;font-size:13px;transition:all .18s;display:flex;align-items:center;justify-content:center;color:var(--txt2)}
.ibt:hover{background:var(--bl)}.ibt.del:hover{background:var(--red-l);color:var(--red)}

/* ── PAGE CONTENT — Zone prospect avec scroll ── */
.page-content{
  padding:20px 24px 40px;
  flex:1;
  overflow-y:auto;
  min-height:0;
  /* Scrollbar élégante */
  scrollbar-width:thin;
  scrollbar-color:var(--bm) transparent;
}
.page-content::-webkit-scrollbar{width:6px}
.page-content::-webkit-scrollbar-track{background:transparent}
.page-content::-webkit-scrollbar-thumb{background:var(--bm);border-radius:10px}
.page-content::-webkit-scrollbar-thumb:hover{background:var(--blue)}

/* Grille avec plus de marge confortable */
.prospects-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(340px,1fr));gap:14px}
.prospects-list{display:flex;flex-direction:column;gap:6px}

/* ── CARDS ── */
.card{background:var(--bg2);border-radius:var(--r4);border:1.5px solid var(--bl);overflow:hidden;transition:all .25s;cursor:pointer;position:relative}
.card:hover{box-shadow:var(--sh3);transform:translateY(-2px);border-color:var(--bm)}
.card-urgency-bar{height:3px;width:100%}
.card-urgency-bar.overdue{background:var(--red)}.card-urgency-bar.today{background:var(--orange)}.card-urgency-bar.soon{background:var(--blue)}.card-urgency-bar.normal{background:var(--bl)}
.card-hdr{padding:16px 18px 12px;display:flex;justify-content:space-between;align-items:flex-start}
.card-name{font-size:15.5px;font-weight:700;margin-bottom:5px;line-height:1.3}
.card-badges{display:flex;gap:6px;flex-wrap:wrap;align-items:center}
.cbadge{padding:3px 9px;border-radius:14px;font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.4px;white-space:nowrap}
.cbadge.organisation{background:var(--green-l);color:var(--green)}.cbadge.negociation{background:var(--blue-l);color:var(--blue)}.cbadge.chaud{background:var(--orange-l);color:var(--orange)}.cbadge.autre{background:var(--purple-l);color:var(--purple)}.cbadge.gagne{background:#dcfce7;color:#16a34a}
.card-acts{display:flex;gap:3px;flex-shrink:0;margin-left:8px}
.card-body{padding:12px 18px 16px}
.ci-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px}
.ci{display:flex;align-items:center;gap:7px;font-size:12px;color:var(--txt2);min-width:0}
.ci-ico{width:24px;height:24px;background:var(--bg3);border-radius:5px;display:flex;align-items:center;justify-content:center;font-size:12px;flex-shrink:0}
.ci span{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.cnotes{font-size:12px;color:var(--txt2);background:var(--bg3);padding:9px 12px;border-radius:var(--r2);margin-bottom:10px;border-left:3px solid var(--bm);display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden}
.abox{padding:12px 14px;border-radius:var(--r3)}
.abox.overdue{background:linear-gradient(135deg,#fff5f5,#fee2e2);border:1px solid #fecaca}
.abox.today{background:linear-gradient(135deg,#fffbeb,#fef3c7);border:1px solid #fde68a}
.abox.soon{background:linear-gradient(135deg,#f0f7ff,#dbeafe);border:1px solid #bfdbfe}
.abox.normal{background:var(--bg3);border:1px solid var(--bl)}
.abox.none{border:2px dashed var(--bl);text-align:center;padding:16px}
.abox-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.aurge{font-size:9px;font-weight:800;text-transform:uppercase;letter-spacing:1px}
.abox.overdue .aurge{color:var(--red)}.abox.today .aurge{color:var(--orange)}.abox.soon .aurge{color:var(--blue)}.abox.normal .aurge{color:var(--txt3)}
.adesc{font-size:13px;font-weight:600;color:var(--txt);margin-bottom:8px;line-height:1.4}
.adate{font-size:11px;font-weight:600;color:var(--txt2)}
.atime{font-size:11px;color:var(--blue);font-weight:600}
.acpl{width:100%;padding:8px;background:#fff;border:1.5px solid var(--green);border-radius:var(--r2);color:var(--green);font-size:12px;font-weight:700;cursor:pointer;transition:all .2s;font-family:inherit}
.acpl:hover{background:var(--green);color:#fff}
.add-action{background:none;border:none;color:var(--blue);font-size:12.5px;font-weight:600;cursor:pointer;display:flex;align-items:center;gap:6px;font-family:inherit}
.add-action:hover{text-decoration:underline}

/* ── LIST ROW ── */
.list-row{background:var(--bg2);border:1.5px solid var(--bl);border-radius:var(--r3);padding:14px 18px;display:grid;grid-template-columns:2fr 1fr 1.5fr 1.2fr 150px auto;gap:14px;align-items:center;cursor:pointer;transition:all .2s;border-left:4px solid transparent}
.list-row:hover{box-shadow:var(--sh2);border-color:var(--bm)}
.list-row.urg-overdue{border-left-color:var(--red)}.list-row.urg-today{border-left-color:var(--orange)}.list-row.urg-soon{border-left-color:var(--blue)}
.lr-name{font-weight:700;font-size:14px}

/* ── EMPTY STATE ── */
.empty{text-align:center;padding:80px 40px;color:var(--txt2)}
.empty-ico{font-size:60px;margin-bottom:18px;opacity:.5}
.empty-title{font-family:'Fraunces',serif;font-size:22px;color:var(--txt);margin-bottom:8px}
.empty-sub{font-size:13.5px;margin-bottom:24px}

.agenda-nav{display:flex;align-items:center;gap:14px;padding:14px 32px;background:var(--bg2);border-bottom:1px solid var(--bl);flex-shrink:0;flex-wrap:wrap}
.week-nav{display:flex;align-items:center;gap:8px}
.week-nav button{padding:7px 12px;border:1.5px solid var(--bl);background:var(--bg2);border-radius:var(--r2);cursor:pointer;font-weight:600;font-size:12px;font-family:inherit;transition:all .2s;color:var(--txt)}
.week-nav button:hover{background:var(--bg3);border-color:var(--bm)}
.week-lbl{font-size:14px;font-weight:700;min-width:190px;text-align:center;color:var(--txt)}
.ag-toggle{display:flex;gap:4px;background:var(--bg3);padding:3px;border-radius:var(--r2);margin-left:auto}
.ag-btn{padding:7px 14px;border:none;background:transparent;border-radius:8px;cursor:pointer;font-size:12.5px;font-weight:600;font-family:inherit;color:var(--txt2);transition:all .2s}
.ag-btn.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.agenda-body{flex:1;overflow-y:auto;overflow-x:auto;padding:0 32px 32px;min-height:0}

/* ── WEEK/DAY GRID ── */
.week-grid{display:grid;grid-template-columns:64px repeat(7,1fr);min-width:860px;border-top:1px solid var(--bl)}
.wg-corner{border-right:1px solid var(--bl);border-bottom:1px solid var(--bl)}
.wg-dayhdr{padding:10px 6px;text-align:center;border-right:1px solid var(--bl);border-bottom:1px solid var(--bl);background:var(--bg3)}
.wg-dayhdr.today{background:var(--blue-l)}
.wg-dayname{font-size:10.5px;font-weight:700;text-transform:uppercase;letter-spacing:.6px;color:var(--txt3)}
.wg-daynum{font-size:20px;font-weight:800;line-height:1.2}
.wg-dayhdr.today .wg-daynum{color:var(--blue)}
.wg-time{padding:0 8px;font-size:10.5px;color:var(--txt3);font-weight:600;height:56px;display:flex;align-items:flex-start;padding-top:5px;border-right:1px solid var(--bl);border-bottom:1px solid var(--bl);background:var(--bg3)}
.wg-cell{border-right:1px solid var(--bl);border-bottom:1px solid var(--bl);height:56px;position:relative;padding:2px;background:#fff}
.wg-cell.today{background:rgba(37,99,235,.02)}
.wg-event{background:var(--blue);color:#fff;border-radius:5px;padding:3px 7px;font-size:10.5px;font-weight:600;cursor:pointer;position:absolute;left:2px;right:2px;top:2px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;transition:all .15s;z-index:2}
.wg-event:hover{opacity:.85;z-index:3;transform:scale(1.01)}
.wg-event.overdue{background:var(--red)}.wg-event.today{background:var(--orange)}.wg-event.soon{background:#3b82f6}.wg-event.normal{background:#64748b}
.wg-add{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;opacity:0;cursor:pointer;font-size:18px;color:var(--txt3);transition:opacity .2s}
.wg-cell:hover .wg-add{opacity:1}
.day-grid{display:grid;grid-template-columns:64px 1fr;min-width:500px;border-top:1px solid var(--bl)}
.day-time{height:56px;display:flex;align-items:flex-start;padding:5px 8px 0;font-size:10.5px;color:var(--txt3);font-weight:600;border-right:1px solid var(--bl);border-bottom:1px solid var(--bl);background:var(--bg3)}
.day-slot{height:56px;border-bottom:1px solid var(--bl);position:relative;padding:3px 5px}
.day-event{background:var(--blue);color:#fff;border-radius:6px;padding:4px 10px;font-size:12px;font-weight:600;cursor:pointer;margin-bottom:2px;display:flex;align-items:center;gap:8px;transition:all .18s}
.day-event:hover{opacity:.88}.day-event.overdue{background:var(--red)}.day-event.today{background:var(--orange)}.day-event.soon{background:#3b82f6}.day-event.normal{background:#64748b}
.cpl-mini{padding:4px 9px;background:rgba(255,255,255,.2);border:1px solid rgba(255,255,255,.4);border-radius:5px;color:#fff;font-size:10.5px;font-weight:700;cursor:pointer;font-family:inherit;flex-shrink:0;transition:all .15s}
.cpl-mini:hover{background:rgba(255,255,255,.35)}

/* ── AGENDA LIST ── */
.ag-list-wrap{padding:20px 0 40px}
.ag-group{margin-bottom:26px}
.ag-group-title{font-size:14px;font-weight:800;padding-bottom:10px;border-bottom:2px solid var(--bl);margin-bottom:10px;display:flex;align-items:center;gap:10px}
.ag-tag{font-size:10px;padding:2px 8px;border-radius:10px;font-weight:700;color:#fff}
.ag-item{background:var(--bg2);border:1.5px solid var(--bl);border-radius:var(--r3);padding:12px 16px;margin-bottom:8px;display:flex;align-items:center;gap:14px;cursor:pointer;transition:all .2s;border-left:4px solid transparent}
.ag-item:hover{box-shadow:var(--sh2);border-color:var(--bm)}
.ag-item.overdue{border-left-color:var(--red)}.ag-item.today{border-left-color:var(--orange)}.ag-item.soon{border-left-color:var(--blue)}
.ag-time{font-size:12.5px;font-weight:700;color:var(--blue);min-width:50px}
.ag-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.ag-prospect{font-weight:700;font-size:13.5px}
.ag-action{font-size:12.5px;color:var(--txt2);margin-top:1px}
.ag-cpl{padding:6px 14px;background:var(--green-l);border:1.5px solid var(--green);border-radius:var(--r2);color:var(--green);font-size:12px;font-weight:700;cursor:pointer;font-family:inherit;white-space:nowrap;transition:all .18s;flex-shrink:0}
.ag-cpl:hover{background:var(--green);color:#fff}

/* ── DETAIL VIEW (fiche client) ── */
.detail-view{display:none;flex:1;overflow-y:auto;flex-direction:column}
.detail-view.open{display:flex}
.dv-hdr{background:linear-gradient(135deg,#1a2340,#1e3a6e);color:#fff;padding:22px 32px;flex-shrink:0}
.dv-back{display:inline-flex;align-items:center;gap:6px;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.2);border-radius:8px;padding:6px 12px;cursor:pointer;font-size:12.5px;font-weight:600;color:#fff;margin-bottom:16px;transition:all .2s;font-family:inherit}
.dv-back:hover{background:rgba(255,255,255,.2)}
.dv-hdr-top{display:flex;justify-content:space-between;align-items:flex-start;gap:16px;margin-bottom:10px}
.dv-name{font-family:'Fraunces',serif;font-size:26px;font-weight:700;margin-bottom:8px;line-height:1.2}
.dv-meta{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.dv-info-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:0;border-top:1px solid rgba(255,255,255,.1);margin-top:16px}
.dv-info-item{padding:14px 16px;border-right:1px solid rgba(255,255,255,.08)}
.dv-info-item:last-child{border-right:none}
.dv-info-lbl{font-size:9.5px;text-transform:uppercase;letter-spacing:.8px;color:rgba(255,255,255,.45);font-weight:700;margin-bottom:5px}
.dv-info-val{font-size:13.5px;font-weight:600;color:#fff;word-break:break-all}
.dv-info-val a{color:#93c5fd;text-decoration:none}.dv-info-val a:hover{text-decoration:underline}
.dv-body{display:grid;grid-template-columns:1fr 350px;gap:20px;padding:24px 32px;flex:1;min-height:0}
.dv-main{display:flex;flex-direction:column;gap:16px;min-height:0;overflow-y:auto}
.dv-aside{display:flex;flex-direction:column;gap:16px}
.panel{background:var(--bg2);border-radius:var(--r4);border:1.5px solid var(--bl);overflow:hidden}
.panel-hdr{padding:14px 20px;background:var(--bg3);border-bottom:1px solid var(--bl);font-size:14px;font-weight:700;display:flex;align-items:center;gap:8px;justify-content:space-between}
.panel-body{padding:20px}

/* ── TIMELINE ── */
.timeline{position:relative;padding-left:34px}
.timeline::before{content:'';position:absolute;left:12px;top:16px;bottom:16px;width:2px;background:var(--bl)}
.tl-item{position:relative;padding-bottom:18px}
.tl-item:last-child{padding-bottom:0}
.tl-dot{position:absolute;left:-34px;top:2px;width:28px;height:28px;background:#fff;border:2.5px solid var(--green);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;z-index:1}
.tl-content{background:var(--bg);border:1px solid var(--bl);border-radius:var(--r2);padding:12px 14px}
.tl-date{font-size:10.5px;color:var(--txt3);font-weight:600;margin-bottom:5px}
.tl-action{font-size:13px;font-weight:600;color:var(--txt);margin-bottom:5px}
.tl-comment{font-size:12px;color:#166534;background:#f0fdf4;border-left:3px solid var(--green);padding:7px 10px;border-radius:4px;margin-top:6px;line-height:1.5}

/* ── ACTION PANEL ── */
.action-panel{background:var(--bg2);border-radius:var(--r4);border:1.5px solid var(--bl);overflow:hidden}
.ap-hdr{padding:14px 20px;background:linear-gradient(135deg,var(--blue),#1d4ed8);color:#fff}
.ap-hdr h3{font-size:14px;font-weight:700;margin-bottom:2px}
.ap-hdr p{font-size:11px;opacity:.75}
.ap-body{padding:18px}
.cur-action{padding:14px 16px;border-radius:var(--r3);margin-bottom:14px;border-left:4px solid var(--blue)}
.cur-action.overdue{border-left-color:var(--red);background:var(--red-l)}
.cur-action.today{border-left-color:var(--orange);background:var(--orange-l)}
.cur-action.soon{border-left-color:var(--blue);background:var(--blue-l)}
.cur-action.normal{border-left-color:var(--bm);background:var(--bg3)}
.ca-lbl{font-size:9.5px;text-transform:uppercase;letter-spacing:1px;color:var(--txt3);font-weight:700;margin-bottom:6px}
.ca-text{font-size:14px;font-weight:700;color:var(--txt);margin-bottom:6px}
.ca-date{font-size:12px;color:var(--txt2);font-weight:600}
.ca-time{font-size:12px;color:var(--blue);font-weight:600;margin-top:3px}
.qa-list{display:flex;flex-direction:column;gap:7px}
.qa-btn{width:100%;padding:11px 14px;border-radius:var(--r2);font-size:13px;font-weight:600;cursor:pointer;transition:all .18s;font-family:inherit;display:flex;align-items:center;gap:8px;border:none}
.qa-btn.complete{background:var(--green-l);border:1.5px solid var(--green);color:var(--green)}.qa-btn.complete:hover{background:var(--green);color:#fff}
.qa-btn.secondary{background:var(--bg3);border:1.5px solid var(--bl);color:var(--txt)}.qa-btn.secondary:hover{background:var(--bl)}
.qa-btn.warn{background:var(--red-l);border:1.5px solid #fecaca;color:var(--red)}.qa-btn.warn:hover{background:var(--red);color:#fff}

/* ── CAT CHANGE PANEL ── */
.cat-grid{display:grid;grid-template-columns:1fr 1fr;gap:6px}
.cat-choice{padding:9px 12px;border:1.5px solid var(--bl);border-radius:var(--r2);cursor:pointer;transition:all .18s;display:flex;align-items:center;gap:7px;font-size:12.5px;font-weight:600;background:var(--bg)}
.cat-choice:hover{border-color:var(--bm);background:var(--bg3)}
.cat-choice.current{border-color:var(--blue);background:var(--blue-l)}

/* ── MODALS ── */
.overlay{position:fixed;inset:0;background:rgba(15,21,38,.55);backdrop-filter:blur(5px);display:none;align-items:center;justify-content:center;z-index:500;padding:20px}
.overlay.open{display:flex}
.modal{background:var(--bg2);border-radius:var(--r5);width:100%;max-width:660px;max-height:94vh;overflow:hidden;box-shadow:var(--sh3);animation:mIn .25s ease}
.modal-sm{max-width:480px}
.modal-md{max-width:560px}
@keyframes mIn{from{opacity:0;transform:translateY(14px) scale(.97)}to{opacity:1;transform:none}}
.modal-hdr{padding:20px 26px;border-bottom:1px solid var(--bl);display:flex;justify-content:space-between;align-items:center;background:var(--bg3);flex-shrink:0}
.modal-title{font-family:'Fraunces',serif;font-size:19px;font-weight:700}
.modal-close{width:36px;height:36px;border:none;background:#fff;border-radius:var(--r2);font-size:20px;cursor:pointer;color:var(--txt2);display:flex;align-items:center;justify-content:center;transition:all .18s}
.modal-close:hover{background:var(--bl);color:var(--txt)}
.modal-body{padding:24px 26px;overflow-y:auto;max-height:calc(94vh - 160px)}
.modal-ftr{padding:16px 26px;border-top:1px solid var(--bl);display:flex;gap:8px;justify-content:flex-end;background:var(--bg3);flex-shrink:0}

/* ── FORM ELEMENTS ── */
.fsec{margin-bottom:22px}
.fsec-title{font-size:13px;font-weight:700;margin-bottom:12px;display:flex;align-items:center;gap:8px;color:var(--txt)}
.fsec-ico{width:24px;height:24px;background:var(--blue-l);color:var(--blue);border-radius:5px;display:flex;align-items:center;justify-content:center;font-size:12px}
.frow{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}
.frow.single{grid-template-columns:1fr}
.fg{display:flex;flex-direction:column;gap:6px}
.fg.full{grid-column:1/-1}
.flbl{font-size:11.5px;font-weight:600;color:var(--txt2)}
.finp,.fsel,.ftxt{padding:11px 13px;border:1.5px solid var(--bl);border-radius:var(--r2);font-size:13px;font-family:inherit;transition:all .2s;background:var(--bg);color:var(--txt);width:100%}
.finp:focus,.fsel:focus,.ftxt:focus{outline:none;border-color:var(--blue);background:#fff;box-shadow:0 0 0 3px rgba(37,99,235,.08)}
.ftxt{resize:vertical;min-height:72px}
.cat-sel{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin-top:4px}
.cat-opt{padding:11px 12px;border:1.5px solid var(--bl);border-radius:var(--r2);cursor:pointer;transition:all .18s;display:flex;align-items:center;gap:7px;font-size:12.5px;font-weight:600;background:var(--bg)}
.cat-opt:hover{border-color:var(--bm)}.cat-opt.selected{border-color:var(--blue);background:var(--blue-l)}
.cat-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.action-type-sel{display:flex;gap:6px;flex-wrap:wrap;margin-top:4px}
.atype{padding:7px 13px;border:1.5px solid var(--bl);border-radius:20px;cursor:pointer;font-size:12px;font-weight:600;background:var(--bg);transition:all .18s;font-family:inherit}
.atype:hover{border-color:var(--bm)}.atype.selected{border-color:var(--blue);background:var(--blue-l);color:var(--blue)}

/* ── DROPDOWN ── */
#prospect-dropdown{display:none;position:absolute;background:#fff;border:1.5px solid var(--bl);border-radius:var(--r2);box-shadow:var(--sh3);z-index:600;max-height:200px;overflow-y:auto;width:100%;left:0;top:calc(100% + 4px)}
.dd-item{padding:10px 14px;cursor:pointer;font-size:13px;border-bottom:1px solid var(--bl)}
.dd-item:last-child{border-bottom:none}
.dd-item:hover{background:var(--bg3)}
.dd-item strong{font-weight:700}
.dd-item small{color:var(--txt3);font-size:11.5px}

/* ── IMPORT ── */
.import-zone{border:2.5px dashed var(--bl);border-radius:var(--r4);padding:44px;text-align:center;cursor:pointer;transition:all .2s;background:var(--bg3)}
.import-zone:hover,.import-zone.drag{border-color:var(--blue);background:var(--blue-l)}
.iz-ico{font-size:44px;margin-bottom:12px}
.iz-title{font-size:15px;font-weight:700;margin-bottom:5px}
.iz-sub{font-size:12.5px;color:var(--txt2);margin-bottom:12px}
.fmt-pill{padding:3px 10px;background:#fff;border:1px solid var(--bl);border-radius:8px;font-size:11px;font-weight:600;margin:0 3px}

/* ── NOTIF ── */
.notif{position:fixed;bottom:26px;right:26px;display:none;align-items:center;gap:10px;padding:13px 18px;background:#fff;border-radius:var(--r3);box-shadow:var(--sh3);border-left:4px solid var(--green);z-index:9999;font-size:13.5px;font-weight:600;min-width:200px}
.notif.error{border-left-color:var(--red)}.notif.warning{border-left-color:var(--orange)}.notif.info{border-left-color:var(--blue)}

/* ── SCORE BADGE ── */
.score-badge{display:inline-flex;align-items:center;gap:4px;padding:3px 9px;border-radius:12px;font-size:10.5px;font-weight:700;background:var(--bg3);border:1px solid var(--bl);color:var(--txt2)}

/* ── RESPONSIVE ── */
@media(max-width:1200px){
  .tb-chip-lbl{display:none}
  .tb-chip{padding:5px 8px}
}
@media(max-width:1100px){
  .dv-body{grid-template-columns:1fr}
  .dv-aside{flex-direction:row;flex-wrap:wrap}
  .dv-aside>*{flex:1;min-width:280px}
}
@media(max-width:960px){
  :root{--sidebar:220px}
  .dv-info-grid{grid-template-columns:repeat(2,1fr)}
  .tb-stats{gap:4px}
}
@media(max-width:760px){
  .tb-stats{display:none}
}
@media(max-width:680px){
  :root{--sidebar:0px}
  .sidebar{display:none}
  .main{margin-left:0}
  .page-content,.agenda-body{padding-left:14px;padding-right:14px}
  .tb-row1,.tb-row2{padding-left:14px;padding-right:14px}
  .frow{grid-template-columns:1fr}
  .app,.main{height:auto;overflow:visible}
  .view.active{height:auto;overflow:visible}
  #dash-content{overflow:visible}
  .page-content{overflow:visible;height:auto;min-height:calc(100vh - 180px)}
}

/* ── SCORE RING ── */
.score-ring{display:flex;align-items:center;gap:8px;font-size:12px;font-weight:600;color:var(--txt2)}
.score-dots{display:flex;gap:3px}
.sdot{width:8px;height:8px;border-radius:50%;background:var(--bl)}
.sdot.on.red{background:var(--red)}.sdot.on.orange{background:var(--orange)}.sdot.on.green{background:var(--green)}

/* ── MODE LECTURE SEULE ── */
body.readonly-mode .readonly-hide{display:none!important}
body.readonly-mode .card:hover{transform:none;cursor:default}
body.readonly-mode .card-acts{display:none!important}
body.readonly-mode .list-row:hover{cursor:default}
.readonly-banner{display:none;background:linear-gradient(135deg,#1e3a6e,#1a2340);color:#fff;padding:11px 32px;font-size:13px;font-weight:600;border-bottom:2px solid #3b82f6;align-items:center;gap:10px;flex-shrink:0}
body.readonly-mode .readonly-banner{display:flex}
.ro-badge{background:#3b82f6;color:#fff;font-size:10px;padding:2px 10px;border-radius:10px;font-weight:800;text-transform:uppercase;letter-spacing:.8px}

/* ── BADGE SAUVEGARDE EN ATTENTE ── */
@keyframes badgePulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.8;transform:scale(1.03)}}

/* ════════════════════════════════════════════
   PAGE D'ACCUEIL — SIPHRO CRM
═══════════════════════════════════════════════ */
#landing{
  position:fixed;inset:0;z-index:9000;
  background:linear-gradient(135deg,#0a0f1e 0%,#0d1a3a 40%,#0f2252 100%);
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  font-family:'Plus Jakarta Sans',sans-serif;
  overflow:hidden;
}
#landing.hidden{display:none}

/* Particules décoratives */
.lp-bg{position:absolute;inset:0;overflow:hidden;pointer-events:none}
.lp-orb{position:absolute;border-radius:50%;filter:blur(80px);opacity:.18}
.lp-orb-1{width:500px;height:500px;background:radial-gradient(circle,#3b82f6,transparent);top:-100px;right:-100px;animation:orbFloat1 12s ease-in-out infinite}
.lp-orb-2{width:400px;height:400px;background:radial-gradient(circle,#8b5cf6,transparent);bottom:-80px;left:-80px;animation:orbFloat2 15s ease-in-out infinite}
.lp-orb-3{width:300px;height:300px;background:radial-gradient(circle,#0891b2,transparent);top:50%;left:50%;transform:translate(-50%,-50%);animation:orbFloat3 10s ease-in-out infinite}
@keyframes orbFloat1{0%,100%{transform:translate(0,0) scale(1)}50%{transform:translate(-30px,30px) scale(1.08)}}
@keyframes orbFloat2{0%,100%{transform:translate(0,0) scale(1)}50%{transform:translate(25px,-25px) scale(1.05)}}
@keyframes orbFloat3{0%,100%{transform:translate(-50%,-50%) scale(1)}50%{transform:translate(-50%,-50%) scale(1.15)}}

/* Grid de points déco */
.lp-grid{position:absolute;inset:0;background-image:radial-gradient(rgba(255,255,255,.07) 1px,transparent 1px);background-size:40px 40px;pointer-events:none}

/* Contenu landing */
.lp-content{position:relative;z-index:1;text-align:center;max-width:820px;padding:40px 24px;animation:lpIn .8s cubic-bezier(.16,1,.3,1) both}
@keyframes lpIn{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}

.lp-logo{display:flex;align-items:center;justify-content:center;gap:14px;margin-bottom:10px}
.lp-logo-icon{width:56px;height:56px;background:linear-gradient(135deg,#3b82f6,#8b5cf6);border-radius:16px;display:flex;align-items:center;justify-content:center;font-size:26px;box-shadow:0 8px 32px rgba(59,130,246,.45);animation:iconPulse 3s ease-in-out infinite}
@keyframes iconPulse{0%,100%{box-shadow:0 8px 32px rgba(59,130,246,.45)}50%{box-shadow:0 12px 48px rgba(59,130,246,.7)}}
.lp-logo-text{font-family:'Fraunces',serif;font-size:38px;font-weight:700;color:#fff;letter-spacing:-.5px}
.lp-tagline{font-size:14px;color:rgba(255,255,255,.4);letter-spacing:2.5px;text-transform:uppercase;margin-bottom:14px;font-weight:600}
.lp-divider{width:60px;height:2px;background:linear-gradient(90deg,transparent,#3b82f6,transparent);margin:0 auto 40px;border-radius:2px}

.lp-title{font-family:'Fraunces',serif;font-size:28px;font-weight:700;color:#fff;margin-bottom:10px;line-height:1.3}
.lp-sub{font-size:14px;color:rgba(255,255,255,.5);margin-bottom:48px;line-height:1.7;max-width:560px;margin-left:auto;margin-right:auto}

/* Cartes de sélection */
.lp-cards{display:grid;grid-template-columns:1fr 1fr;gap:20px;width:100%;max-width:700px;margin:0 auto}
.lp-card{
  background:rgba(255,255,255,.04);
  border:1.5px solid rgba(255,255,255,.1);
  border-radius:20px;
  padding:32px 28px;
  cursor:pointer;
  transition:all .3s cubic-bezier(.16,1,.3,1);
  text-align:left;
  position:relative;
  overflow:hidden;
  text-decoration:none;
  display:block;
}
.lp-card::before{
  content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(255,255,255,.04),transparent);
  opacity:0;transition:opacity .3s;
}
.lp-card:hover{
  transform:translateY(-6px);
  border-color:rgba(255,255,255,.25);
  box-shadow:0 24px 60px rgba(0,0,0,.5);
}
.lp-card:hover::before{opacity:1}
.lp-card.admin{border-color:rgba(59,130,246,.35)}
.lp-card.admin:hover{border-color:rgba(59,130,246,.7);box-shadow:0 24px 60px rgba(59,130,246,.25)}
.lp-card.collab{border-color:rgba(139,92,246,.3)}
.lp-card.collab:hover{border-color:rgba(139,92,246,.65);box-shadow:0 24px 60px rgba(139,92,246,.2)}

.lp-card-ico{
  width:52px;height:52px;border-radius:14px;
  display:flex;align-items:center;justify-content:center;
  font-size:22px;margin-bottom:20px;
}
.lp-card.admin .lp-card-ico{background:linear-gradient(135deg,rgba(59,130,246,.25),rgba(37,99,235,.15));border:1px solid rgba(59,130,246,.4)}
.lp-card.collab .lp-card-ico{background:linear-gradient(135deg,rgba(139,92,246,.25),rgba(109,40,217,.15));border:1px solid rgba(139,92,246,.4)}

.lp-card-title{font-size:18px;font-weight:800;color:#fff;margin-bottom:8px;display:flex;align-items:center;gap:10px}
.lp-card-badge{font-size:9px;padding:3px 9px;border-radius:8px;font-weight:800;text-transform:uppercase;letter-spacing:.8px}
.lp-card.admin .lp-card-badge{background:rgba(59,130,246,.25);color:#93c5fd;border:1px solid rgba(59,130,246,.4)}
.lp-card.collab .lp-card-badge{background:rgba(139,92,246,.2);color:#c4b5fd;border:1px solid rgba(139,92,246,.35)}

.lp-card-desc{font-size:12.5px;color:rgba(255,255,255,.5);line-height:1.7;margin-bottom:22px}
.lp-card-features{list-style:none}
.lp-card-features li{font-size:12px;color:rgba(255,255,255,.4);padding:4px 0;display:flex;align-items:center;gap:7px}
.lp-card-features li::before{content:'';width:5px;height:5px;border-radius:50%;flex-shrink:0}
.lp-card.admin .lp-card-features li::before{background:#3b82f6}
.lp-card.collab .lp-card-features li::before{background:#8b5cf6}

.lp-card-arrow{
  position:absolute;bottom:24px;right:24px;
  width:36px;height:36px;border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  font-size:16px;transition:all .3s;
  opacity:0;transform:translateX(-6px);
}
.lp-card.admin .lp-card-arrow{background:rgba(59,130,246,.3)}
.lp-card.collab .lp-card-arrow{background:rgba(139,92,246,.3)}
.lp-card:hover .lp-card-arrow{opacity:1;transform:translateX(0)}

.lp-footer{
  margin-top:40px;
  font-size:11px;color:rgba(255,255,255,.2);
  letter-spacing:.8px;text-transform:uppercase;
  display:flex;align-items:center;gap:12px;
}
.lp-footer span{width:1px;height:10px;background:rgba(255,255,255,.15);display:inline-block}

/* Stats mini dans le landing */
.lp-stats{display:flex;gap:28px;justify-content:center;margin-bottom:44px}
.lp-stat{text-align:center}
.lp-stat-val{font-family:'Fraunces',serif;font-size:28px;font-weight:700;color:#fff;line-height:1}
.lp-stat-lbl{font-size:10px;color:rgba(255,255,255,.3);text-transform:uppercase;letter-spacing:1px;margin-top:3px;font-weight:600}
.lp-stat-sep{width:1px;background:rgba(255,255,255,.1);align-self:stretch;margin:4px 0}

@media(max-width:600px){
  .lp-cards{grid-template-columns:1fr}
  .lp-logo-text{font-size:28px}
  .lp-title{font-size:22px}
  .lp-stats{gap:18px}
  .lp-stat-val{font-size:22px}
}
</style>
</head>
<body>

<!-- ══════════════════════════════════════════════
     PAGE D'ACCUEIL SIPHRO CRM
     Masquée automatiquement si ?mode=admin ou ?readonly=1
═══════════════════════════════════════════════ -->
<div id="landing">
  <div class="lp-bg">
    <div class="lp-orb lp-orb-1"></div>
    <div class="lp-orb lp-orb-2"></div>
    <div class="lp-orb lp-orb-3"></div>
    <div class="lp-grid"></div>
  </div>

  <div class="lp-content">
    <div class="lp-logo">
      <div class="lp-logo-icon">◈</div>
      <div class="lp-logo-text">SIPHRO</div>
    </div>
    <div class="lp-tagline">CRM Commercial • CHR Méditerranée</div>
    <div class="lp-divider"></div>

    <div class="lp-stats">
      <div class="lp-stat">
        <div class="lp-stat-val">52</div>
        <div class="lp-stat-lbl">Prospects</div>
      </div>
      <div class="lp-stat-sep"></div>
      <div class="lp-stat">
        <div class="lp-stat-val" id="lp-chaud-count">—</div>
        <div class="lp-stat-lbl">Dossiers chauds</div>
      </div>
      <div class="lp-stat-sep"></div>
      <div class="lp-stat">
        <div class="lp-stat-val" id="lp-actions-count">—</div>
        <div class="lp-stat-lbl">Actions en cours</div>
      </div>
    </div>

    <div class="lp-title">Choisissez votre mode d'accès</div>
    <div class="lp-sub">Sélectionnez votre profil pour accéder au suivi commercial. L'accès administrateur permet les modifications, l'accès équipe est en lecture seule.</div>

    <div class="lp-cards">
      <!-- ADMIN -->
      <a href="#" class="lp-card admin" id="lp-admin-btn">
        <div class="lp-card-ico">⚡</div>
        <div class="lp-card-title">
          Administrateur
          <span class="lp-card-badge">Complet</span>
        </div>
        <div class="lp-card-desc">Accès total au CRM. Créez, modifiez et gérez l'intégralité des dossiers prospects.</div>
        <ul class="lp-card-features">
          <li>Ajouter / modifier des prospects</li>
          <li>Planifier et compléter les actions</li>
          <li>Importer & exporter les données</li>
          <li>Accès agenda et suivi complet</li>
        </ul>
        <div class="lp-card-arrow">→</div>
      </a>

      <!-- COLLABORATEUR -->
      <a href="#" class="lp-card collab" id="lp-collab-btn">
        <div class="lp-card-ico">👁️</div>
        <div class="lp-card-title">
          Équipe / Collaborateur
          <span class="lp-card-badge">Lecture</span>
        </div>
        <div class="lp-card-desc">Consultez les dossiers et le planning commercial sans pouvoir modifier les données.</div>
        <ul class="lp-card-features">
          <li>Consultation de tous les prospects</li>
          <li>Accès agenda et planning</li>
          <li>Export CSV autorisé</li>
          <li>Aucune modification possible</li>
        </ul>
        <div class="lp-card-arrow">→</div>
      </a>
    </div>

    <div class="lp-footer">
      <span>SIPHRO</span>
      <span></span>
      <span>CRM v3.0</span>
      <span></span>
      <span>CHR Méditerranée</span>
      <span></span>
      <span id="lp-date"></span>
    </div>
  </div>
</div>

<div class="app">

<!-- ══ SIDEBAR ══ -->
<aside class="sidebar">
  <div class="sb-brand">
    <div class="sb-brand-row">
      <div class="sb-icon">◈</div>
      <div><div class="sb-title">SIPHRO</div><div class="sb-sub">CRM Commercial</div></div>
    </div>
    <div class="sb-sync" id="sb-sync">🔄 Chargement...</div>
  </div>
  <nav class="sb-nav">
    <div class="sb-sec">
      <div class="sb-sec-lbl">Navigation</div>
      <div class="ni active" id="ni-dashboard" onclick="showView('dashboard')"><span class="ni-ico">📊</span><span class="ni-lbl">Tableau de bord</span></div>
      <div class="ni" id="ni-agenda" onclick="showView('agenda')"><span class="ni-ico">📅</span><span class="ni-lbl">Agenda</span><span class="nbadge b-blue" id="sb-agenda-count">0</span></div>
    </div>
    <div class="sb-sec">
      <div class="sb-sec-lbl">Catégories</div>
      <div class="ni" onclick="applyFilter('organisation')"><span class="dot" style="background:#059669"></span><span class="ni-lbl">En organisation</span><span class="nbadge b-green" id="cnt-organisation">0</span></div>
      <div class="ni" onclick="applyFilter('negociation')"><span class="dot" style="background:#2563eb"></span><span class="ni-lbl">En négociation</span><span class="nbadge b-blue" id="cnt-negociation">0</span></div>
      <div class="ni" onclick="applyFilter('chaud')"><span class="dot" style="background:#d97706"></span><span class="ni-lbl">Dossiers chauds</span><span class="nbadge b-orange" id="cnt-chaud">0</span></div>
      <div class="ni" onclick="applyFilter('autre')"><span class="dot" style="background:#7c3aed"></span><span class="ni-lbl">Autres à suivre</span><span class="nbadge b-gray" id="cnt-autre">0</span></div>
      <div class="ni" onclick="applyFilter('gagne')"><span class="dot" style="background:#16a34a"></span><span class="ni-lbl">Dossiers gagnés</span><span class="nbadge b-green" id="cnt-gagne">0</span></div>
    </div>
    <div class="sb-sec">
      <div class="sb-sec-lbl">Alertes</div>
      <div class="ni" onclick="applyFilter('overdue')" style="color:#f87171"><span class="ni-ico">🔴</span><span class="ni-lbl">En retard</span><span class="nbadge" id="cnt-overdue">0</span></div>
      <div class="ni" onclick="applyFilter('today')" style="color:#fbbf24"><span class="ni-ico">🟡</span><span class="ni-lbl">Aujourd'hui</span><span class="nbadge b-orange" id="cnt-today">0</span></div>
      <div class="ni" onclick="applyFilter('week')" style="color:#60a5fa"><span class="ni-ico">🔵</span><span class="ni-lbl">Cette semaine</span><span class="nbadge b-blue" id="cnt-week">0</span></div>
    </div>
    <div class="sb-sec">
      <div class="sb-sec-lbl">Données</div>
      <div class="ni readonly-hide" onclick="openImportModal()"><span class="ni-ico">📥</span><span class="ni-lbl">Importer CSV/Excel</span></div>
      <div class="ni" onclick="exportData()"><span class="ni-ico">📤</span><span class="ni-lbl">Exporter CSV</span></div>
      <div class="ni readonly-hide" onclick="openDeletedModal()"><span class="ni-ico">🗑️</span><span class="ni-lbl">Prospects supprimés</span><span class="nbadge b-gray" id="deleted-count">0</span></div>
      <div class="ni readonly-hide" onclick="showBackupInfo()" title="Vérifier la sauvegarde locale"><span class="ni-ico">🔒</span><span class="ni-lbl">Sauvegarde locale</span></div>
    </div>
  </nav>
  <div class="sb-footer"><div class="sb-version">SIPHRO CRM v2.1 — CHR Méditerranée<br><span style="color:rgba(255,255,255,.15);font-size:9px">🔒 Données protégées localement</span></div></div>
</aside>

<!-- ══ MAIN ══ -->
<main class="main">

<!-- Bandeau mode lecture seule (affiché uniquement en mode ?readonly=1) -->
<div class="readonly-banner">
  <span style="font-size:18px">👁️</span>
  <span>Mode <span class="ro-badge">Consultation</span></span>
  <span style="color:rgba(255,255,255,.6);font-size:12px">— Vous consultez le CRM SIPHRO en lecture seule. Aucune modification n'est possible.</span>
  <span style="margin-left:auto;color:rgba(255,255,255,.4);font-size:11px">SIPHRO CRM • CHR Méditerranée</span>
</div>

<!-- ── DASHBOARD VIEW ── -->
<div class="view active" id="view-dashboard">
  <!-- Fiche client (detail view) à l'intérieur du dashboard -->
  <div id="detail-view" class="detail-view"></div>
  <!-- Contenu principal dashboard -->
  <div id="dash-content">

    <!-- ══ TOP BAR COMPACT (remplace header + stats + filtres) ══ -->
    <div class="top-bar">
      <!-- Ligne 1 : titre + chips stats + actions -->
      <div class="tb-row1">
        <div class="tb-title-wrap">
          <div class="tb-title">Prospects</div>
          <div class="tb-sub">CHR Méditerranée</div>
        </div>
        <!-- Mini stats chips cliquables -->
        <div class="tb-stats">
          <div class="tb-chip" id="chip-total" onclick="applyFilter('all')" title="Tous les prospects">
            <span class="tb-chip-val" id="stat-total">0</span>
            <span class="tb-chip-lbl">Total</span>
          </div>
          <div class="tb-chip c-red" id="chip-overdue" onclick="applyFilter('overdue')" title="En retard">
            <span>🔴</span>
            <span class="tb-chip-val" id="stat-retard">0</span>
            <span class="tb-chip-lbl">Retard</span>
          </div>
          <div class="tb-chip c-orange" id="chip-today" onclick="applyFilter('today')" title="Action aujourd'hui">
            <span>🟡</span>
            <span class="tb-chip-val" id="stat-today">0</span>
            <span class="tb-chip-lbl">Auj.</span>
          </div>
          <div class="tb-chip c-green" id="chip-nego" onclick="applyFilter('negociation')" title="En négociation">
            <span>🤝</span>
            <span class="tb-chip-val" id="stat-nego">0</span>
            <span class="tb-chip-lbl">Négo</span>
          </div>
          <div class="tb-chip" id="chip-orga" onclick="applyFilter('organisation')" title="En organisation">
            <span>📋</span>
            <span class="tb-chip-val" id="stat-orga">0</span>
            <span class="tb-chip-lbl">Orga</span>
          </div>
        </div>
        <!-- Actions -->
        <div class="tb-actions">
          <span class="sync-pill" id="sync-badge">🔄</span>
          <span id="unsaved-badge" style="display:none;font-size:11px;font-weight:700;padding:5px 10px;background:#fef3c7;border:1.5px solid #f59e0b;border-radius:16px;color:#92400e;cursor:pointer;animation:badgePulse 2s ease-in-out infinite" onclick="pushToSheets()" title="Cliquer pour forcer la sync maintenant"></span>
          <button class="btn btn-secondary btn-sm readonly-hide" onclick="openImportModal()">📥</button>
          <button class="btn btn-primary btn-sm readonly-hide" onclick="openProspectModal()">＋ Prospect</button>
        </div>
      </div>
      <!-- Ligne 2 : recherche + filtres + tri + vue -->
      <div class="tb-row2">
        <div class="search-wrap">
          <span class="si">🔍</span>
          <input type="text" id="search-input" placeholder="Rechercher un prospect, contact, ville, email…" oninput="handleSearch(this.value)" autocomplete="off">
          <button class="clear-btn" onclick="clearSearch()">×</button>
        </div>
        <div class="ftabs" id="filter-tabs">
          <button class="ftab active" onclick="setFilter('all')" data-f="all">Tous <span class="cnt" id="fc-all">0</span></button>
          <button class="ftab" onclick="setFilter('organisation')" data-f="organisation">📋 <span class="cnt" id="fc-organisation">0</span></button>
          <button class="ftab" onclick="setFilter('negociation')" data-f="negociation">🤝 <span class="cnt" id="fc-negociation">0</span></button>
          <button class="ftab" onclick="setFilter('chaud')" data-f="chaud">🔥 <span class="cnt" id="fc-chaud">0</span></button>
          <button class="ftab" onclick="setFilter('autre')" data-f="autre">📌 <span class="cnt" id="fc-autre">0</span></button>
          <button class="ftab" onclick="setFilter('gagne')" data-f="gagne">✅ <span class="cnt" id="fc-gagne">0</span></button>
        </div>
        <select class="sort-sel" id="sort-select" onchange="renderProspects()">
          <option value="urgency">🔥 Urgence</option>
          <option value="name">🔤 Nom A→Z</option>
          <option value="date-asc">📅 Date ↑</option>
          <option value="date-desc">📅 Date ↓</option>
          <option value="recent">🕐 Récent</option>
          <option value="hist">📊 + d'actions</option>
        </select>
        <div class="view-tog">
          <button class="vtb active" id="vt-grid" onclick="setViewMode('grid')" title="Grille">⊞</button>
          <button class="vtb" id="vt-list" onclick="setViewMode('list')" title="Liste">☰</button>
        </div>
      </div>
    </div>

    <!-- Zone de scroll prospects -->
    <div class="page-content" id="page-content">
      <div id="prospects-container"></div>
      <div class="empty" id="empty-state" style="display:none">
        <div class="empty-ico">📂</div>
        <h2 class="empty-title">Aucun prospect trouvé</h2>
        <p class="empty-sub">Modifiez votre recherche ou ajoutez un nouveau prospect</p>
        <div style="display:flex;gap:10px;justify-content:center">
          <button class="btn btn-secondary btn-sm" onclick="clearSearch()">🔄 Effacer la recherche</button>
          <button class="btn btn-primary btn-sm readonly-hide" onclick="openProspectModal()">+ Ajouter</button>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- ── AGENDA VIEW ── -->
<div class="view" id="view-agenda">
  <div class="top-bar">
    <div class="tb-row1" style="padding:10px 24px">
      <div class="tb-title-wrap">
        <div class="tb-title">Agenda</div>
        <div class="tb-sub">Actions planifiées</div>
      </div>
      <div class="tb-stats" style="justify-content:flex-start">
        <div class="week-nav">
          <button onclick="agNav(-1)" style="padding:6px 10px;border:1.5px solid var(--bl);background:var(--bg);border-radius:8px;cursor:pointer;font-weight:600;font-size:12px;font-family:inherit;transition:all .2s;color:var(--txt)">← Préc</button>
          <div class="week-lbl" id="ag-label" style="font-size:13px;font-weight:700;min-width:170px;text-align:center;color:var(--txt)">...</div>
          <button onclick="agNav(1)" style="padding:6px 10px;border:1.5px solid var(--bl);background:var(--bg);border-radius:8px;cursor:pointer;font-weight:600;font-size:12px;font-family:inherit;transition:all .2s;color:var(--txt)">Suiv →</button>
          <button onclick="agToday()" style="padding:6px 10px;background:var(--blue-l);color:var(--blue);border:1.5px solid var(--blue);border-radius:8px;cursor:pointer;font-weight:700;font-size:12px;font-family:inherit">Aujourd'hui</button>
        </div>
      </div>
      <div class="tb-actions">
        <span class="sync-pill" id="sync-badge2">✅ Sync</span>
        <div class="ag-toggle" style="display:flex;gap:3px;background:var(--bg3);padding:3px;border-radius:8px">
          <button class="ag-btn" id="ag-list" onclick="setAgView('list')" style="padding:6px 12px;border:none;background:transparent;border-radius:6px;cursor:pointer;font-size:12px;font-weight:600;font-family:inherit;color:var(--txt2);transition:all .2s">📋 Liste</button>
          <button class="ag-btn active" id="ag-week" onclick="setAgView('week')" style="padding:6px 12px;border:none;background:#fff;border-radius:6px;cursor:pointer;font-size:12px;font-weight:600;font-family:inherit;color:var(--txt);box-shadow:var(--sh1);transition:all .2s">📅 Semaine</button>
          <button class="ag-btn" id="ag-day" onclick="setAgView('day')" style="padding:6px 12px;border:none;background:transparent;border-radius:6px;cursor:pointer;font-size:12px;font-weight:600;font-family:inherit;color:var(--txt2);transition:all .2s">🕐 Jour</button>
        </div>
        <button class="btn btn-primary btn-sm readonly-hide" onclick="openActionModal(null,true)">+ Tâche</button>
      </div>
    </div>
  </div>
  <div class="agenda-body" id="agenda-content"></div>
</div>

</main>
</div>

<!-- ══ MODALS ══ -->

<!-- Nouveau / Modifier prospect -->
<div class="overlay" id="prospect-modal">
  <div class="modal">
    <div class="modal-hdr"><h2 class="modal-title" id="modal-title">Nouveau prospect</h2><button class="modal-close" onclick="closeProspectModal()">×</button></div>
    <div class="modal-body">
      <form id="prospect-form" onsubmit="handleProspectSubmit(event)">
        <input type="hidden" id="prospect-id">
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">🏢</span>Établissement</div>
          <div class="frow">
            <div class="fg"><label class="flbl">Nom établissement *</label><input type="text" class="finp" id="f-nom" required placeholder="Ex: Le Matchico, Camping Les Pins..."></div>
            <div class="fg"><label class="flbl">SIRET</label><input type="text" class="finp" id="f-siret" placeholder="14 chiffres"></div>
          </div>
          <div class="fg full frow"><label class="flbl">Adresse / Ville</label><input type="text" class="finp" id="f-adresse" placeholder="Montpellier, Agde, Sète..."></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">👤</span>Contact principal</div>
          <div class="frow">
            <div class="fg"><label class="flbl">Nom du contact</label><input type="text" class="finp" id="f-contact-nom" placeholder="Prénom Nom"></div>
            <div class="fg"><label class="flbl">Téléphone</label><input type="tel" class="finp" id="f-contact-tel" placeholder="06 XX XX XX XX"></div>
          </div>
          <div class="fg full frow"><label class="flbl">Email</label><input type="email" class="finp" id="f-contact-email" placeholder="contact@exemple.fr"></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">📊</span>Classification & Formation</div>
          <div class="cat-sel" id="cat-sel">
            <div class="cat-opt" data-v="organisation" onclick="selectCat('organisation')"><span class="cat-dot" style="background:#059669"></span>📋 En organisation</div>
            <div class="cat-opt" data-v="negociation" onclick="selectCat('negociation')"><span class="cat-dot" style="background:#2563eb"></span>🤝 En négociation</div>
            <div class="cat-opt selected" data-v="chaud" onclick="selectCat('chaud')"><span class="cat-dot" style="background:#d97706"></span>🔥 Dossier chaud</div>
            <div class="cat-opt" data-v="autre" onclick="selectCat('autre')"><span class="cat-dot" style="background:#7c3aed"></span>📌 Autre à suivre</div>
            <div class="cat-opt" data-v="gagne" onclick="selectCat('gagne')"><span class="cat-dot" style="background:#16a34a"></span>✅ Dossier gagné</div>
          </div>
          <div class="fg" style="margin-top:12px"><label class="flbl">Formation visée</label><input type="text" class="finp" id="f-formation" placeholder="Ex: HACCP, Management, Pâtisserie..."></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">📝</span>Notes & Contexte</div>
          <div class="fg full"><textarea class="ftxt" id="f-commentaire" rows="3" placeholder="Contexte du contact, besoins spécifiques, observations importantes..."></textarea></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">🎯</span>Prochaine action (optionnel)</div>
          <div class="frow single"><div class="fg"><label class="flbl">Type d'action</label>
            <div class="action-type-sel" id="f-atype-sel">
              <button type="button" class="atype" data-v="📞 Appel" onclick="selAtype(this)">📞 Appel</button>
              <button type="button" class="atype" data-v="📧 Email" onclick="selAtype(this)">📧 Email</button>
              <button type="button" class="atype" data-v="🤝 RDV" onclick="selAtype(this)">🤝 RDV</button>
              <button type="button" class="atype" data-v="📋 Devis" onclick="selAtype(this)">📋 Devis</button>
              <button type="button" class="atype" data-v="📌 Relance" onclick="selAtype(this)">📌 Relance</button>
            </div>
            <input type="text" class="finp" id="f-action-desc" placeholder="Description de l'action..." style="margin-top:8px">
          </div></div>
          <div class="frow">
            <div class="fg"><label class="flbl">Date</label><input type="date" class="finp" id="f-action-date"></div>
            <div class="fg"><label class="flbl">Heure début</label><input type="time" class="finp" id="f-action-hstart"></div>
          </div>
          <div class="frow">
            <div class="fg"><label class="flbl">Heure fin</label><input type="time" class="finp" id="f-action-hend"></div>
            <div class="fg"><label class="flbl">Assigné à</label><input type="text" class="finp" id="f-action-assignee" placeholder="Collaborateur"></div>
          </div>
        </div>
      </form>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeProspectModal()">Annuler</button>
      <button class="btn btn-primary" onclick="document.getElementById('prospect-form').dispatchEvent(new Event('submit',{cancelable:true,bubbles:true}))">✓ Enregistrer</button>
    </div>
  </div>
</div>

<!-- Planifier action -->
<div class="overlay" id="action-modal">
  <div class="modal modal-md">
    <div class="modal-hdr"><h2 class="modal-title" id="action-modal-title">Planifier une action</h2><button class="modal-close" onclick="closeActionModal()">×</button></div>
    <div class="modal-body">
      <form id="action-form" onsubmit="handleActionSubmit(event)">
        <input type="hidden" id="action-prospect-id">
        <div class="fg" style="margin-bottom:14px;position:relative">
          <label class="flbl">Prospect *</label>
          <input type="text" class="finp" id="action-prospect-search" placeholder="Tapez le nom du prospect..." oninput="searchProspectDD(this.value)" autocomplete="off">
          <div id="prospect-dropdown"></div>
        </div>
        <div class="fg" style="margin-bottom:4px"><label class="flbl">Type d'action</label>
          <div class="action-type-sel" id="action-atype-sel">
            <button type="button" class="atype" data-v="📞 Appel" onclick="selAtypeAction(this)">📞 Appel</button>
            <button type="button" class="atype" data-v="📧 Email" onclick="selAtypeAction(this)">📧 Email</button>
            <button type="button" class="atype" data-v="🤝 RDV" onclick="selAtypeAction(this)">🤝 RDV</button>
            <button type="button" class="atype" data-v="📋 Devis" onclick="selAtypeAction(this)">📋 Devis</button>
            <button type="button" class="atype" data-v="📌 Relance" onclick="selAtypeAction(this)">📌 Relance</button>
          </div>
        </div>
        <div class="fg" style="margin-bottom:14px"><label class="flbl">Description *</label><input type="text" class="finp" id="action-desc" required placeholder="Ex: Appeler pour confirmer les dates de formation"></div>
        <div class="frow">
          <div class="fg"><label class="flbl">Date *</label><input type="date" class="finp" id="action-date" required></div>
          <div class="fg"><label class="flbl">Heure début</label><input type="time" class="finp" id="action-hstart"></div>
        </div>
        <div class="frow">
          <div class="fg"><label class="flbl">Heure fin</label><input type="time" class="finp" id="action-hend"></div>
          <div class="fg"><label class="flbl">Assigné à</label><input type="text" class="finp" id="action-assignee" placeholder="Collaborateur"></div>
        </div>
        <div class="fg" style="margin-top:10px"><label class="flbl">Notes préparatoires</label><textarea class="ftxt" id="action-notes" rows="2" placeholder="Points à aborder, informations utiles..."></textarea></div>
      </form>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeActionModal()">Annuler</button>
      <button class="btn btn-primary" onclick="document.getElementById('action-form').dispatchEvent(new Event('submit',{cancelable:true,bubbles:true}))">📅 Planifier</button>
    </div>
  </div>
</div>

<!-- Compléter action avec compte-rendu -->
<div class="overlay" id="complete-modal">
  <div class="modal modal-sm">
    <div class="modal-hdr"><h2 class="modal-title">✅ Confirmer l'action</h2><button class="modal-close" onclick="closeCompleteModal()">×</button></div>
    <div class="modal-body">
      <div style="background:var(--bg3);border-radius:var(--r3);padding:14px 16px;margin-bottom:18px;border-left:4px solid var(--green)">
        <div style="font-size:10px;text-transform:uppercase;letter-spacing:1px;color:var(--txt3);font-weight:700;margin-bottom:5px">Prospect</div>
        <div style="font-size:14px;font-weight:700;color:var(--txt)" id="cpl-prospect-name"></div>
        <div style="font-size:12.5px;color:var(--txt2);margin-top:4px" id="cpl-action-desc"></div>
      </div>
      <div class="fg" style="margin-bottom:14px">
        <label class="flbl">💬 Compte-rendu de l'action</label>
        <textarea class="ftxt" id="cpl-comment" rows="4" placeholder="Résumé de l'appel, email envoyé, résultat de la réunion, prochaines étapes...&#10;Ce commentaire sera visible dans l'historique du dossier."></textarea>
      </div>
      <div class="fg">
        <label class="flbl">📅 Planifier la prochaine action (optionnel)</label>
        <div style="display:flex;gap:8px;margin-top:4px">
          <input type="text" class="finp" id="cpl-next-desc" placeholder="Prochaine action..." style="flex:2">
          <input type="date" class="finp" id="cpl-next-date" style="flex:1">
        </div>
      </div>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeCompleteModal()">Annuler</button>
      <button class="btn btn-green" onclick="confirmCompleteAction()">✓ Confirmer et enregistrer</button>
    </div>
  </div>
</div>

<!-- Import -->
<div class="overlay" id="import-modal">
  <div class="modal">
    <div class="modal-hdr"><h2 class="modal-title">📥 Importer des contacts</h2><button class="modal-close" onclick="closeImportModal()">×</button></div>
    <div class="modal-body">
      <div class="import-zone" id="import-zone" onclick="document.getElementById('file-input').click()">
        <div class="iz-ico">📁</div>
        <div class="iz-title">Glissez votre fichier ici</div>
        <div class="iz-sub">ou cliquez pour sélectionner</div>
        <div><span class="fmt-pill">CSV</span><span class="fmt-pill">Excel .xlsx</span></div>
      </div>
      <input type="file" id="file-input" accept=".csv,.xlsx,.xls" style="display:none" onchange="handleFileSelect(event)">
      <div style="margin-top:18px;padding:16px;background:var(--bg3);border-radius:var(--r3)">
        <h4 style="font-size:13px;font-weight:700;margin-bottom:8px">📋 Colonnes attendues</h4>
        <div style="background:#fff;padding:10px;border-radius:8px;font-family:monospace;font-size:11px;overflow-x:auto;white-space:nowrap;border:1px solid var(--bl)">nom_etablissement ; contact_nom ; contact_telephone ; contact_email ; adresse ; siret ; categorie ; formation_visee ; commentaire ; prochaine_action ; date_prochaine_action</div>
      </div>
    </div>
    <div class="modal-ftr"><button class="btn btn-secondary" onclick="closeImportModal()">Fermer</button></div>
  </div>
</div>

<!-- Prospects supprimés -->
<div class="overlay" id="deleted-modal">
  <div class="modal" style="max-width:900px">
    <div class="modal-hdr">
      <h2 class="modal-title">🗑️ Historique des suppressions</h2>
      <button class="modal-close" onclick="closeDeletedModal()">×</button>
    </div>
    <div class="modal-body" style="max-height:70vh;overflow-y:auto">
      <div id="deleted-list"></div>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeDeletedModal()">Fermer</button>
      <button class="btn btn-danger" onclick="clearDeletedHistory()" title="Vider définitivement l'historique">🗑️ Vider l'historique</button>
    </div>
  </div>
</div>

<!-- Notif toast -->
<div class="notif" id="notif"><span id="notif-ico">✓</span><span id="notif-text"></span></div>

<script>
// ══════════════════════════════════════════════
//  SIPHRO CRM v2.0 — Core JavaScript
// ══════════════════════════════════════════════

const SCRIPT_URL='https://script.google.com/macros/s/AKfycbyn_49XC_MOUfCt714IP_oSxhGNGUn9n1M6xm5AsdHiWKN-XRdeS8cCQAcSuG8v0dXMnA/exec';

// ══════════════════════════════════════════════
// 🔒 BASE COMPLÈTE — 52 PROSPECTS SIPHRO
// Source: Google Sheets (21) + Salon Manquants (30) + Extra (1)
// Protection: ces prospects ne peuvent PAS être écrasés ni supprimés automatiquement
// Fusion intelligente par nom (dédoublonnage automatique)
// ══════════════════════════════════════════════
const ALL_PROSPECTS_SEED=[
  // ─── GOOGLE SHEETS — 21 PROSPECTS ───────────────────────────────────────────
  {nom:'Thermes de Balaruc',contactNom:'Pôle formation',contactTel:'-',contactEmail:'poleformation.poleformation@thermesbalaruc.com',adresse:'Balaruc',siret:'',categorie:'autre',formation:'Ouverture Resto',commentaire:'PRIORITÉ. Création restaurant. Se positionner.',prochaineAction:{description:'RELANCE MAIL 2 (chercher numéro tel)',date:'2026-03-03',hstart:'09:00',hend:'09:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Premier contact au salon'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail de relance envoyé après salon'}]},
  {nom:'Cadence au Musée',contactNom:'À TROUVER',contactTel:'',contactEmail:'',adresse:'Narbonne',siret:'',categorie:'autre',formation:'',commentaire:'⚠️ AUCUNE INFO — Contact à trouver',prochaineAction:{description:'RECHERCHER CONTACT + RELANCE',date:'2026-02-19',hstart:'10:00',hend:'10:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Contact rapide au salon'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail envoyé mais pas de réponse — aucun contact trouvé'}]},
  {nom:'Ambrussum',contactNom:'Monsieur',contactTel:'04 67 86 76 73',contactEmail:'restaurantambrussum@gmail.com',adresse:'Villetelle',siret:'',categorie:'autre',formation:'',commentaire:"17/02 : J'échange avec l'associé de la personne vue au salon qui m'informe qu'ils ont prévu formation en salle (sommellerie) et en cuisine (cuisson sous vide/basse temp). N'a pas d'autres besoins actuellement, accepte le suivi et de recevoir un mail.",prochaineAction:{description:'SUIVIT COURANT ANNÉE 2026 (BUDGET UTILISÉ)',date:'2026-05-04',hstart:'09:00',hend:'09:30',createdAt:'2026-02-17T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Contact salon — formations en cours'},{description:'RELANCE MAIL APRÉS SALON + Échange avec associé',date:'2026-02-17',createdAt:'2026-02-17T10:00:00.000Z',commentaire:"Budget OPCO déjà utilisé pour l'année — formations programmées. Suivi en mai 2026."}]},
  {nom:'Randstad Mtp',contactNom:'Sylvia Chastang',contactTel:'04 67 99 81 35 / 06 16 17 95 26',contactEmail:'sylvia.chastang@randstadsearch.fr',adresse:'Montpellier',siret:'',categorie:'autre',formation:'',commentaire:'Gère RH agence. Fait intervenir formations CPF/autres. Potentiellement intéressée. ⚠️ MAIL PAS ENVOYÉ',prochaineAction:null,actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Discussion RH et formations'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'⚠️ Attention: mail PAS envoyé — à envoyer'}]},
  {nom:'Camping Corse',contactNom:'LUCCHINI TOUSSAINT',contactTel:'',contactEmail:'lucchinitoussaint@gmail.com',adresse:'Corse',siret:'',categorie:'chaud',formation:'',commentaire:'Intéressé formation finance.',prochaineAction:{description:'RELANCE MAIL 2 (chercher numéro tel)',date:'2026-02-20',hstart:'09:00',hend:'09:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Intéressé formation finance'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail envoyé — pas de réponse — chercher numéro tel'}]},
  {nom:'Restaurant République Coricala',contactNom:'Optistanal',contactTel:'',contactEmail:'',adresse:'Vias',siret:'',categorie:'autre',formation:'Restauration',commentaire:'Madame pressée mais intéressée. Recontact prévu.',prochaineAction:{description:'RAPPEL SUITE NRP',date:'2026-02-18',hstart:'10:00',hend:'10:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Madame pressée mais intéressée'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'NRP — à rappeler'}]},
  {nom:'BBQ',contactNom:'',contactTel:'06 59 34 21 18',contactEmail:'traiteurbbq@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'Pas de locaux ?',prochaineAction:{description:'RELANCE SALON',date:'2026-02-16',hstart:'14:00',hend:'14:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[]},
  {nom:'RESTAURANT NIMES (PROJET)',contactNom:'',contactTel:'',contactEmail:'david.terlecki@orange.fr',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'Explication OPCO — Projet ouverture restaurant traditionnel',prochaineAction:{description:'Relance mail Salon',date:'2026-02-16',hstart:'14:30',hend:'15:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[]},
  {nom:'TRAITEUR',contactNom:'',contactTel:'',contactEmail:'viccos66450@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'3 personnes familiale — Ouvert depuis 3 ans — Plusieurs points à former',prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-16',hstart:'15:00',hend:'15:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[]},
  {nom:'Miozotis - Future Pizzeria',contactNom:'Messieurs Dames',contactTel:'',contactEmail:'',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:"Projet achat pizzeria. De base événementiel. Projet en cours. À suivre.",prochaineAction:{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-16',hstart:'15:30',hend:'16:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Projet pizzeria en cours — à suivre'}]},
  {nom:'OCCITANIA REPUBLIC',contactNom:'Chloé Calas',contactTel:'06 34 30 43 73',contactEmail:'oliana-34@hotmail.com',adresse:'Vias',siret:'',categorie:'chaud',formation:'',commentaire:"Pressée mais intéressée — Restaurant sur Vias\nFixe : 09.52.22.21.60\n17/02 : NRP",prochaineAction:{description:'RAPPEL SUITE NRP',date:'2026-02-18',hstart:'09:30',hend:'10:00',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'RELANCE SUITE SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Première relance — NRP le 17/02'}]},
  {nom:'La Table de Fah',contactNom:'Madame',contactTel:'07 49 47 80 88',contactEmail:'contact@latabledefah.fr',adresse:'Mèze',siret:'',categorie:'chaud',formation:'Hygiène (HACCP)',commentaire:'Ne connaissait pas les budgets. Besoin d\'accompagnement.',prochaineAction:{description:'Rappel — voir si reprise effectuée pour relancer sur la formation',date:'2026-03-09',hstart:'09:00',hend:'09:30',createdAt:'2026-02-17T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Premier contact — besoin HACCP'},{description:'Relance mail après salon',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail envoyé avec catalogue'},{description:'ÉCHANGE TEL — REPRISE EN COURS — INTÉRESSÉ — ON SE RECONTACTE',date:'2026-02-17',createdAt:'2026-02-17T10:00:00.000Z',commentaire:'Reprise en cours — rappel prévu fin mars pour lancer la formation HACCP'}]},
  {nom:'Bella Pizza',contactNom:'Monsieur',contactTel:'-',contactEmail:'jfjt@hotmail.fr',adresse:'Sommières',siret:'',categorie:'chaud',formation:'-',commentaire:'Pizzeria + restaurant. Intérêt pour la solution.',prochaineAction:{description:'RELANCE MAIL 2 (chercher numéro tel)',date:'2026-02-21',hstart:'09:00',hend:'09:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Contact salon'},{description:'RELANCE SUITE ÉCHANGE SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail envoyé — pas de réponse — pas de tel'}]},
  {nom:'Le Matchico',contactNom:'Johanna Khoury',contactTel:'06 85 79 02 99',contactEmail:'lematchico@gmail.com',adresse:'Saint-Affrique',siret:'',categorie:'chaud',formation:'Hygiène (HACCP)',commentaire:"Entreprise familiale, quatre personnes. Ont eu un contrôle d'hygiène récemment. OK pour repasser la formation.",prochaineAction:{description:'Recontacter suite au premier échange',date:'2026-03-03',hstart:'09:30',hend:'10:00',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Besoin HACCP — contrôle récent'},{description:'Mail envoyé après salon',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Catalogue envoyé'},{description:'Relance téléphonique',date:'2026-02-17',createdAt:'2026-02-17T10:00:00.000Z',commentaire:'Mr débordé — pas eu le temps de regarder — recontacter début mars'},{description:'ÉCHAN TEL — MR DÉBORDÉ — PAS EU LE TEMPS DE S\'Y PENCHER — À RECONTACTER',date:'2026-02-19',createdAt:'2026-02-19T10:00:00.000Z',commentaire:'Rappel programmé au 03/03'}]},
  {nom:'Judy',contactNom:'Madame',contactTel:'04 34 00 57 54',contactEmail:'contact@judy-mauguio.fr',adresse:'Mauguio',siret:'',categorie:'chaud',formation:'Hygiène équipe',commentaire:'Restaurant asiatique — pas d\'hygiène sauf madame — intéressé pour hygiène équipe.',prochaineAction:{description:'RAPPEL — CAR PATRONNE ABSENTE LORS DE LA RELANCE',date:'2026-02-19',hstart:'10:30',hend:'11:00',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Restaurant asiatique — hygiène équipe'},{description:'RELANCE APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Patronne absente — à rappeler'}]},
  {nom:'Korean Korner',contactNom:'M. Liberbaum',contactTel:'06 25 02 42 24',contactEmail:'l.liberbaum@korean-korner.com',adresse:'Montpellier',siret:'',categorie:'chaud',formation:'Management / Hygiène',commentaire:"Pleine expansion sur Marseille, ouvre son troisième établissement. Besoin de formation managers.\n17/02 : N'a pas encore vu le catalogue, Mr dit vouloir me rappeler, je relance dans 2 semaines",prochaineAction:{description:'RELANCE TEL — SUITE ÉCHANGE — PAS VU CATALOGUE ENCORE',date:'2026-03-03',hstart:'10:00',hend:'10:30',createdAt:'2026-02-17T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Expansion — 3ème établissement — management'},{description:'Mail avec catalogue',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Catalogue envoyé'},{description:'RELANCE SUITE MAIL SALON — Échange tel',date:'2026-02-17',createdAt:'2026-02-17T10:00:00.000Z',commentaire:"N'a pas encore vu le catalogue — rappel dans 2 semaines"}]},
  {nom:'La Guinguette du Ponant',contactNom:'Henri Dupré',contactTel:'06 60 10 92 52',contactEmail:'henridupre@live.fr',adresse:'La Grande Motte',siret:'',categorie:'chaud',formation:'HYGIÈNE ET À VOIR',commentaire:'Venu au salon pour chercher partenaire formation. La Grande Motte triple d\'effectif en saison. À recontacter.',prochaineAction:{description:'Relance suite à Message vocal',date:'2026-02-20',hstart:'11:00',hend:'11:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon — venu chercher partenaire',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Motivé — triple effectif en saison'},{description:'RELANCE APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Message vocal laissé — à rappeler'}]},
  {nom:'Camping Les Sablons',contactNom:'Chef de cuisine',contactTel:'-',contactEmail:'chef@lessablons.com',adresse:'-',siret:'',categorie:'chaud',formation:'CDI / Équipe',commentaire:'80 personnes sur la saison. Vient de signer un CDI. Intéressé par la formation.',prochaineAction:{description:'RELANCE MAIL 2 (chercher numéro tel)',date:'2026-02-20',hstart:'11:30',hend:'12:00',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'80 personnes saison — CDI signé'},{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Mail envoyé'},{description:'Appel camping — RESTAURANT FERMÉ — FAIRE MAIL',date:'2026-02-17',createdAt:'2026-02-17T10:00:00.000Z',commentaire:'Restaurant camping fermé hors saison — contact par mail uniquement'}]},
  {nom:'Tropicana',contactNom:'Valérie Delaire',contactTel:'06 20 80 43 00',contactEmail:'tropicana.pv@wanadoo.fr',adresse:'Porto-Vecchio',siret:'',categorie:'chaud',formation:'Formation sur place',commentaire:'Intéressée formation sur place. 7 personnes en cuisine.',prochaineAction:{description:'RAPPEL SUITE MESSAGE VOCAL',date:'2026-02-19',hstart:'14:00',hend:'14:30',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Formation sur place — 7 pers cuisine'},{description:'RELANCE APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'Message vocal laissé'}]},
  {nom:'Le Soleou',contactNom:'Yohan Jullien',contactTel:'06 23 15 19 16',contactEmail:'maximefabre@hotmail.fr',adresse:'Grau-du-Roi',siret:'',categorie:'chaud',formation:'-',commentaire:'Venu chercher des infos. À relancer.',prochaineAction:{description:'RELANCE 2 SUITE NRP',date:'2026-02-20',hstart:'14:30',hend:'15:00',createdAt:'2026-02-19T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'À relancer'},{description:'RELANCE APRÉS SALON',date:'2026-02-14',createdAt:'2026-02-14T10:00:00.000Z',commentaire:'NRP — 2ème relance à faire'}]},
  {nom:'Les Copains à Table',contactNom:'Christian Carensac',contactTel:'04 26 17 61 42',contactEmail:'fournisseurs@lescopainsatable.com',adresse:'Canet (66140)',siret:'',categorie:'chaud',formation:'-',commentaire:'Mise en relation par Pierre. Intéressé.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-16',hstart:'15:00',hend:'15:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mise en relation par Pierre — mail envoyé'}]},
  // ─── POKE AVENUE (Agenda) ────────────────────────────────────────────────────
  {nom:'Poke Avenue',contactNom:'Monsieur',contactTel:'-',contactEmail:'',adresse:'',siret:'',categorie:'organisation',formation:'',commentaire:'RDV planifié lundi 16/02 à 13h30',prochaineAction:{description:'RDV LUNDI 13H30',date:'2026-02-16',hstart:'13:30',hend:'14:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[]},
  // ─── PROSPECTS MANQUANTS SALON — 30 PROSPECTS ───────────────────────────────
  {nom:'French Kiss Mtp',contactNom:'Hédi Khodja',contactTel:'06 29 79 50 87',contactEmail:'dir.montpellier@eklohotels.com',adresse:'Montpellier',siret:'',categorie:'chaud',formation:'Intra (2 pers)',commentaire:'Lien avec 12 hôtels Eklo. Très intéressé. Ne trouve pas de formation intra pour deux personnes.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-17',hstart:'09:00',hend:'09:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Hôtel Le Bon Port / La Balette',contactNom:'Isabelle Amoros & Karine Giraux',contactTel:'06 03 47 68 81',contactEmail:'croquesoleil@gmail.com',adresse:'Collioure (66190)',siret:'',categorie:'chaud',formation:'Catalogue',commentaire:'Double contact. Ne connaissaient pas les budgets. Intéressées.',prochaineAction:{description:'RELANCE TEL APRÉS SALON + MAIL',date:'2026-02-18',hstart:'10:00',hend:'10:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon + MAIL AVEC CATALOGUE',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon + MAIL AVEC CATALOGUE (2026-02-12)'}]},
  {nom:'Camping Les Coudoulets',contactNom:'Clément Pouzache',contactTel:'04 75 93 94 95',contactEmail:'camping@coudoulets.com',adresse:'Pradons (07120)',siret:'',categorie:'autre',formation:'Cuisine / Rentabilité',commentaire:"Très intéressé. Formation cuisine. Parle d'un autre budget OPCO dédié au camping.",prochaineAction:{description:'RELANCE SUITE SALON (BUDGET CAMPING ?)',date:'2026-02-16',hstart:'14:00',hend:'14:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Bar Tapas',contactNom:'Jérémie',contactTel:'-',contactEmail:'jeremycrt14@gmail.com',adresse:'Port Camargue',siret:'',categorie:'autre',formation:'',commentaire:'ECHANGE RAPIDE - Ne connaît pas OPCO - ÉQUIPE EN PLACE',prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-16',hstart:'14:30',hend:'15:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'SAS MPDP',contactNom:'JEREMY SACCOCCIO',contactTel:'06 46 42 00 21',contactEmail:'sasmpdp@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'',prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-16',hstart:'15:00',hend:'15:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[]},
  {nom:'SARL Chez Ttiou',contactNom:'Stéphane Baeza',contactTel:'06 24 37 03 03',contactEmail:'stephqn.baeza@cegetel.net',adresse:'34140',siret:'',categorie:'autre',formation:'',commentaire:'',prochaineAction:{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-18',hstart:'10:30',hend:'11:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Tour Lumas',contactNom:'Employé',contactTel:'',contactEmail:'',adresse:'Arles',siret:'',categorie:'autre',formation:'',commentaire:'Centrale avec chef livre 4 restaurants ville. Peu chance transmission. Pas mail/contact.',prochaineAction:{description:'RECHERCHE + RELANCE',date:'2026-02-18',hstart:'11:00',hend:'11:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Domaine Le Caylou',contactNom:'Madame Legrand Estelle',contactTel:'06 13 07 58 96',contactEmail:'evenements.caylou@gmail.com',adresse:'30122',siret:'',categorie:'autre',formation:'',commentaire:'',prochaineAction:{description:'RELANCE MAIL APRÉS SALON',date:'2026-02-18',hstart:'09:30',hend:'10:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Café des Arts',contactNom:'Florian Mentheour',contactTel:'-',contactEmail:'florian.mentheour@orange.fr',adresse:'',siret:'',categorie:'autre',formation:'Catalogue',commentaire:'Salarié mais grosse équipe en haute saison, je fais le suivi quand même.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'14:00',hend:'14:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'La Galère',contactNom:'Monsieur',contactTel:'04 67 32 15 25',contactEmail:'lagalere34@gmail.com',adresse:'Valras-Plage',siret:'',categorie:'autre',formation:'GLACIER (À VOIR SI CATALOGUE)',commentaire:'Prise de contact positive. Pas de mail envoyé car pas de catalogue glacier.',prochaineAction:{description:'MAIL CATALOGUE GLACIER SUITE ÉCHANGE SALON',date:'2026-02-18',hstart:'15:00',hend:'15:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Camping Les Amandiers',contactNom:'Madame',contactTel:'',contactEmail:'camping-lesamandiers@orange.fr',adresse:'',siret:'',categorie:'autre',formation:'Snacking / Gestion',commentaire:'Connaît OPCO mais trop compliqué. Intéressée solution. Accepte catalogue + recontact.',prochaineAction:{description:'RELANCE SUITE MAIL SALON',date:'2026-02-18',hstart:'16:00',hend:'16:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Traiteur Sannath',contactNom:'Mesdames Cortell',contactTel:'-',contactEmail:'traiteur.sannath@gmail.com',adresse:'Serignan',siret:'',categorie:'autre',formation:'',commentaire:'Clos des Oliviers. Ne connaissaient pas les budgets.',prochaineAction:{description:'RELANCE SUITE MAIL SALON',date:'2026-02-18',hstart:'16:30',hend:'17:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'La Régence',contactNom:'Madame',contactTel:'-',contactEmail:'elodiealais1981@gmail.com',adresse:'Alès',siret:'',categorie:'autre',formation:'Recrutement / HACCP',commentaire:'Possible embauche avant saison.',prochaineAction:{description:'RELANCE SUITE ÉCHANGE SALON',date:'2026-02-18',hstart:'09:00',hend:'09:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'La Sergerie',contactNom:'',contactTel:'-',contactEmail:'Lasergeriefrontdemer@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'Catalogue',commentaire:'Pensait devoir financer. Intéressée après explication.',prochaineAction:{description:'RELANCE SUITE ÉCHANGE SALON',date:'2026-02-18',hstart:'09:30',hend:'10:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'La Plage',contactNom:'Alain Bourrer',contactTel:'-',contactEmail:'alainbourrer@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'En vente. Accepte le suivi mais je ne pense pas que cela intéressera Mr car en vente.',prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-18',hstart:'10:00',hend:'10:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Le Maya',contactNom:'Jyanne Boulila',contactTel:'-',contactEmail:'gihane_boulila@hotmail.fr',adresse:'Ste-Marie-de-la-Mer',siret:'',categorie:'autre',formation:'Brunch / Hygiène',commentaire:"Équipe de 4. Ne connaît pas les budgets.",prochaineAction:{description:'RELANCE SUITE ÉCHANGE SALON',date:'2026-02-18',hstart:'11:00',hend:'11:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Restaurant Le 9',contactNom:'Monsieur',contactTel:'-',contactEmail:'Julienb1204@gmail.com',adresse:'Béziers',siret:'',categorie:'autre',formation:'',commentaire:"Intéressé par la formation, connaît Acto. Pas le temps d'échanger mais fait preuve d'intérêt. Prend la carte et la plaquette.",prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'14:00',hend:'14:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Brasserie Le Carola',contactNom:'Christian Guiot',contactTel:'06 12 26 25 14',contactEmail:'christian.guiot30@sfr.fr',adresse:'Uzès',siret:'',categorie:'autre',formation:'',commentaire:'Ne connaissait pas les budgets. Intéressé.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'14:30',hend:'15:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Brasserie La Détente',contactNom:'Employé',contactTel:'-',contactEmail:'Cameloubimbo@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'Anglais',commentaire:'Employé intéressé. Doit en parler aux gérants.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'15:00',hend:'15:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Aux Terrasses',contactNom:'Damien Latour',contactTel:'06 47 98 42 44',contactEmail:'damienmaster80@hotmail.fr',adresse:'Nîmes',siret:'',categorie:'autre',formation:'Cuisine (4 pers)',commentaire:'Ne connaissaient pas les budgets. Intéressés.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'15:30',hend:'16:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Ponant sur Berges',contactNom:'Vincent Vignon',contactTel:'-',contactEmail:'PONANTSURBERGES@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'Échange rapide. Directeur intéressé.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'16:00',hend:'16:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'La Fontaine aux Artistes',contactNom:'Le Roy Vincent',contactTel:'-',contactEmail:'leroyvincent34@gmail.com',adresse:'Gignac',siret:'',categorie:'autre',formation:'Pâtisserie',commentaire:'Connaît le système des budgets OPCO. A déjà réalisé une formation pâtisserie. Peut être intéressé par notre solution. Accepte le catalogue.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'16:30',hend:'17:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Catalogue envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Catalogue envoyé (2026-02-12)'}]},
  {nom:'Le Sahuc',contactNom:'Farangouise',contactTel:'-',contactEmail:'Farenqluis@gmail.com',adresse:'Rivière-sur-Tarn',siret:'',categorie:'autre',formation:'Catalogue',commentaire:'Quatre personnes + lui = cinq. Ne connaissait pas les budgets OPCO. Intéressé mais ne pense pas forcément avoir besoin de se former. Regarde le catalogue pour réfléchir.',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'17:00',hend:'17:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'SAS Les Filles',contactNom:'Mesdames',contactTel:'06 32 05 70 38',contactEmail:'infoschezlesfilles@gmail.com',adresse:'Lozère',siret:'',categorie:'autre',formation:'',commentaire:"2 personnes uniquement - voir si HCR ou rapide - une des deux a déjà fait l'hygiène récemment.",prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'11:30',hend:'12:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé (2026-02-12)'}]},
  {nom:'Projet Resto Nîmes',contactNom:'Le May Émilien',contactTel:'-',contactEmail:'Lemayemilien@gmail.com',adresse:'Nîmes',siret:'',categorie:'autre',formation:'Catalogue',commentaire:'Projet en cours, pas encore ouvert. Intéressé par suivi.',prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-18',hstart:'10:30',hend:'11:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Restaurant À la Moule',contactNom:'Abdesadek Kacimi',contactTel:'07 67 60 06 28',contactEmail:'kacimixx@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'Accompagnement',commentaire:"Ne connais pas le fonctionnement des budgets OPCO. J'explique notre solution et les budgets. Peuvent être intéressés.",prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-18',hstart:'09:00',hend:'09:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Les Filles du 15',contactNom:'Céline Alboussière',contactTel:'06 62 82 71 58',contactEmail:'artesia14@yahoo.fr',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:"Projet d'ouverture de restauration, deux personnes dans un premier temps. Pas de salariés. Ne connaissaient pas les budgets. Peuvent être intéressées.",prochaineAction:{description:'RELANCE SUIVIT SALON',date:'2026-02-18',hstart:'10:00',hend:'10:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Plaquette donnée',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Plaquette donnée (2026-02-12)'}]},
  {nom:'SAS Côté Jardin Gordes',contactNom:'Juliette Lambert',contactTel:'07 69 97 60 60',contactEmail:'juliette.lambert19899@hotmail.com',adresse:'Gordes / Vaucluse',siret:'',categorie:'autre',formation:'Accompagnement',commentaire:"Madame n'est pas la décisionnaire. Je lui parle des budgets qu'elle ne connaissait pas. Intéressée par l'accompagnement et la démarche.",prochaineAction:{description:'RELANCE SUITE SALON',date:'2026-02-18',hstart:'10:30',hend:'11:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Échange Salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Échange Salon (2026-02-12)'}]},
  {nom:'Boulangerie Jessica',contactNom:'Jessica Poisseau',contactTel:'-',contactEmail:'Jessica.poso34@gmail.com',adresse:'',siret:'',categorie:'autre',formation:'',commentaire:'',prochaineAction:{description:'RELANCE APRÉS SALON',date:'2026-02-23',hstart:'09:00',hend:'09:30',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:'Mail envoyé après salon',date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:'Mail envoyé après salon (2026-02-12)'}]},
  {nom:'Au Pain du Rhony',contactNom:'Monsieur',contactTel:'04 66 35 01 23',contactEmail:'Aupaindurhony@gmail.com',adresse:'Aimargues (Gard)',siret:'',categorie:'autre',formation:'?',commentaire:"Équipe 20 pers. Connaît déjà les budgets OPCO.",prochaineAction:{description:'RELANCE APRÉS SALON + MAIL',date:'2026-02-23',hstart:'09:30',hend:'10:00',createdAt:'2026-02-12T10:00:00.000Z'},actionsEffectuees:[{description:"Échange Salon + mail d'après salon",date:'2026-02-12',createdAt:'2026-02-12T10:00:00.000Z',commentaire:"Échange Salon + mail d'après salon (2026-02-12)"}]}
];

// Alias pour compatibilité (la fusion se fait sur ALL_PROSPECTS_SEED)
const MISSING_PROSPECTS_SEED = ALL_PROSPECTS_SEED;
const CAT={
  organisation:{l:'Organisation',i:'📋',c:'#059669'},
  negociation:{l:'Négociation',i:'🤝',c:'#2563eb'},
  chaud:{l:'Dossier chaud',i:'🔥',c:'#d97706'},
  autre:{l:'À suivre',i:'📌',c:'#7c3aed'},
  gagne:{l:'Gagné',i:'✅',c:'#16a34a'}
};
const HOURS=['08:00','09:00','10:00','11:00','12:00','13:00','14:00','15:00','16:00','17:00','18:00','19:00'];
const DAYS_FR=['Dim','Lun','Mar','Mer','Jeu','Ven','Sam'];
const MONTHS_FR=['janvier','février','mars','avril','mai','juin','juillet','août','septembre','octobre','novembre','décembre'];

// ── STATE ──
let prospects=[];
let deletedProspects=[]; // 🗑️ TRAÇABILITÉ DES SUPPRESSIONS
let currentFilter='all';
let filterMode='cat'; // 'cat' | 'urgency'
let searchQuery='';
let selectedCat='chaud';
let viewMode='grid';
let currentView='dashboard';
let agView='week';
let agOffset=0;
let agDayOffset=0;
let isSaving=false;
let pendingSave=false;
let pendingSaveTimer=null;
let saveRetryCount=0;
const MAX_RETRY=4;
let lastSuccessfulSheetsSync=null;
let unsavedChanges=0;
const LS_KEY="siphro_v5_data";
const LS_PENDING_KEY="siphro_v5_pending";
let syncTimer=null;
let detailOpen=false;
let completePid=null;

// ══════════════════════════════════════════════
// 🔒 MOTEUR DE SYNC V5 — FIABLE
// localStorage = SOURCE DE VÉRITÉ PRINCIPALE
// Google Sheets = miroir secondaire avec retry
// ══════════════════════════════════════════════

// ── INDICATEUR DE STATUT ──
function setSyncStatus(msg,color,pending){
  ['sb-sync','sync-badge','sync-badge2'].forEach(id=>{
    const e=document.getElementById(id);
    if(e){
      e.textContent=msg;
      if(color)e.style.color=color;
      e.style.fontWeight=pending?'800':'600';
    }
  });
}

// ── SAUVEGARDE LOCALE IMMÉDIATE (toujours synchrone) ──
function saveLocalNow(){
  try{
    localStorage.setItem(LS_KEY,JSON.stringify(prospects));
    localStorage.setItem(LS_KEY+'_date',new Date().toISOString());
    // Rétro-compatibilité avec l'ancienne clé
    localStorage.setItem('siphro_prospects_backup',JSON.stringify(prospects));
    localStorage.setItem('siphro_backup_date',new Date().toISOString());
  }catch(e){
    console.warn('localStorage plein ou bloqué:',e);
  }
}
// Alias pour compatibilité avec les appels existants
function saveLocalBackup(){saveLocalNow();}

// ── CHARGER DEPUIS LOCAL ──
function loadFromLocal(){
  try{
    const v5=localStorage.getItem(LS_KEY);
    if(v5){const d=JSON.parse(v5);if(d&&d.length>0)return d;}
    // Fallback ancienne clé
    const old=localStorage.getItem('siphro_prospects_backup');
    if(old){const d=JSON.parse(old);if(d&&d.length>0)return d;}
  }catch(e){}
  return[];
}

// ── CHARGER LES DONNÉES ──
async function loadData(){
  setSyncStatus('🔄 Chargement...','#d97706',false);

  // ÉTAPE 1 : charger immédiatement depuis local (zéro délai pour l'utilisateur)
  const localData=loadFromLocal();
  if(localData.length>0){
    prospects=localData.map(enrichProspect);
    mergeMissingProspects(false); // fusion sans re-save immédiat
    renderAll();
    setSyncStatus(`📱 Local (${prospects.length})`,'#7c3aed',false);
  }

  // ÉTAPE 2 : essayer de charger depuis Sheets en arrière-plan
  try{
    const ctrl=new AbortController();
    const timeout=setTimeout(()=>ctrl.abort(),8000); // 8s max
    const res=await fetch(SCRIPT_URL+'?t='+Date.now(),{
      method:'GET',mode:'cors',cache:'no-cache',
      headers:{'Accept':'application/json'},
      signal:ctrl.signal
    });
    clearTimeout(timeout);
    if(!res.ok)throw new Error('HTTP '+res.status);
    const raw=await res.json();

    // Vérifier que c'est bien un tableau (pas un objet d'erreur)
    if(!Array.isArray(raw)){
      if(raw.error)throw new Error('Sheets: '+raw.error);
      throw new Error('Réponse inattendue');
    }

    const remote=raw.map(parseRow).filter(p=>p.nom).map(enrichProspect);

    // RÈGLE : ne jamais accepter moins de prospects que ce qu'on a localement
    // (protection contre un Sheet vide accidentellement)
    if(remote.length>0 && remote.length>=Math.max(1,prospects.length-5)){
      // Sheets a plus ou autant de données → utiliser Sheets
      prospects=remote;
      mergeMissingProspects(false);
      saveLocalNow(); // Mettre à jour le local avec les données fraîches
      renderAll();
      setSyncStatus(`✅ Synchronisé (${prospects.length})`,'#059669',false);
      lastSuccessfulSheetsSync=Date.now();
    }else if(remote.length>0 && remote.length<prospects.length){
      // Sheets a MOINS de prospects que local → local est plus complet
      // Déclencher une re-sync immédiate pour mettre Sheets à jour
      console.warn(`⚠️ Sheets(${remote.length}) < Local(${prospects.length}) — push local → Sheets`);
      setSyncStatus(`⚠️ Sync en cours...`,'#d97706',true);
      await pushToSheets();
    }else{
      // Sheets vide et local vide → situation initiale normale
      mergeMissingProspects(false);
      saveLocalNow();
      renderAll();
      setSyncStatus(`✅ Prêt (${prospects.length})`,'#059669',false);
    }

  }catch(e){
    // Sheets inaccessible — les données locales sont déjà chargées, on continue
    const reason=e.name==='AbortError'?'Timeout':'Hors ligne';
    console.warn(`Sheets inaccessible (${reason}):`,e.message);
    if(localData.length>0){
      setSyncStatus(`📱 ${reason} — Local OK`,'#7c3aed',false);
    }else{
      setSyncStatus('⚠️ Sans connexion','#d97706',false);
    }
  }

  // Démarrer l'auto-refresh (60s — plus doux)
  if(!syncTimer)syncTimer=setInterval(autoRefresh,60000);
}

// ── FUSION PROSPECTS INITIAUX ──
function mergeMissingProspects(andSave=true){
  const existingNames=new Set(prospects.map(p=>p.nom.toLowerCase().trim()));
  let added=0;
  MISSING_PROSPECTS_SEED.forEach(seed=>{
    const nameKey=seed.nom.toLowerCase().trim();
    if(!existingNames.has(nameKey)){
      const np={...seed,id:genId(),createdAt:seed.createdAt||new Date().toISOString()};
      np.actionsEffectuees=seed.actionsEffectuees||[];
      prospects.push(enrichProspect(np));
      existingNames.add(nameKey);
      added++;
    }
  });
  if(added>0){
    console.log(`✅ ${added} prospects fusionnés`);
    if(andSave)scheduleSheetsSave();
  }
}

// ══════════════════════════════════════════════
// 🔒 MOTEUR DE SAUVEGARDE VERS SHEETS
// Corrige les 3 bugs critiques :
// Bug 1 : mode:'no-cors' → remplacé par cors + vérification réelle
// Bug 2 : autoRefresh écrasait les modifs → bloqué pendant sauvegarde
// Bug 3 : pas de retry → 4 tentatives avec délai exponentiel
// ══════════════════════════════════════════════

// ENTRÉE PRINCIPALE : appelée par toute modification utilisateur
function saveData(){
  if(document.body.classList.contains('readonly-mode'))return;

  // 1. SAUVEGARDE LOCALE IMMÉDIATE — toujours, synchrone, fiable
  saveLocalNow();
  unsavedChanges++;
  updateSaveIndicator();

  // 2. DEBOUNCE : attendre 800ms d'inactivité avant d'envoyer à Sheets
  //    (évite d'envoyer 10 requêtes pour une seule action complexe)
  if(pendingSaveTimer)clearTimeout(pendingSaveTimer);
  pendingSaveTimer=setTimeout(()=>{
    scheduleSheetsSave();
  },800);
}

// Planifier l'envoi vers Sheets
function scheduleSheetsSave(){
  if(isSaving){
    // Déjà en cours → replanifier après fin
    pendingSave=true;
    return;
  }
  pushToSheets();
}

// Envoi réel vers Google Sheets avec retry
async function pushToSheets(){
  if(document.body.classList.contains('readonly-mode'))return;
  isSaving=true;
  setSyncStatus('💾 Sync Sheets...','#d97706',true);

  const snapshot=JSON.parse(JSON.stringify(prospects)); // copie figée au moment de l'envoi
  let success=false;

  for(let attempt=1;attempt<=MAX_RETRY;attempt++){
    try{
      const ctrl=new AbortController();
      const timeout=setTimeout(()=>ctrl.abort(),10000);

      const res=await fetch(SCRIPT_URL,{
        method:'POST',
        // ✅ FIX BUG 1 : mode:'cors' au lieu de 'no-cors'
        // Avec no-cors on ne peut jamais savoir si ça a marché
        mode:'cors',
        cache:'no-cache',
        headers:{'Content-Type':'application/json','Accept':'application/json'},
        body:JSON.stringify({action:'save_all',data:snapshot}),
        signal:ctrl.signal
      });
      clearTimeout(timeout);

      // Lire la réponse pour vérifier que Apps Script a bien accepté
      const text=await res.text().catch(()=>'');
      const isOk=text.startsWith('ok:')||text==='ok';

      if(isOk){
        success=true;
        unsavedChanges=0;
        saveRetryCount=0;
        lastSuccessfulSheetsSync=Date.now();
        updateSaveIndicator();
        setSyncStatus(`✅ Sauvegardé (${snapshot.length})`,'#059669',false);
        console.log(`✅ Sheets: ${snapshot.length} prospects sauvegardés (tentative ${attempt})`);
        break;
      }else{
        throw new Error(`Apps Script a répondu: ${text.substring(0,80)}`);
      }

    }catch(e){
      const isTimeout=e.name==='AbortError';
      const isCors=e.message.includes('CORS')||e.message.includes('Failed to fetch');
      console.warn(`⚠️ Tentative ${attempt}/${MAX_RETRY} échouée:`,e.message);

      if(attempt<MAX_RETRY){
        // Délai exponentiel : 1s, 2s, 4s
        const delay=Math.pow(2,attempt-1)*1000;
        setSyncStatus(`🔄 Retry ${attempt+1}/${MAX_RETRY}...`,'#d97706',true);
        await new Promise(r=>setTimeout(r,delay));
      }else{
        // Toutes les tentatives ont échoué
        saveRetryCount++;
        setSyncStatus(`💾 Local ✓ (Sheets ✗)`,'#7c3aed',true);
        // Marquer comme "en attente" pour retry au prochain autoRefresh
        try{localStorage.setItem(LS_PENDING_KEY,'1');}catch(e2){}

        if(isCors){
          showNotif('⚠️ Problème CORS — vérifiez le déploiement Apps Script','warning');
        }else if(isTimeout){
          showNotif('⏱️ Timeout Sheets — données sauvegardées localement','warning');
        }else{
          showNotif('💾 Données sauvegardées localement. Retry à la prochaine action.','info');
        }
      }
    }
  }

  isSaving=false;

  // S'il y avait un save en attente pendant qu'on sauvegardait → le faire maintenant
  if(pendingSave){
    pendingSave=false;
    saveLocalNow(); // Re-sauvegarder le local (peut avoir changé)
    setTimeout(()=>pushToSheets(),500);
  }
}

// Indicateur visuel de modifications non confirmées
function updateSaveIndicator(){
  const badge=document.getElementById('unsaved-badge');
  if(!badge)return;
  if(unsavedChanges>0){
    badge.style.display='flex';
    badge.textContent=`💾 ${unsavedChanges} modif. en attente`;
  }else{
    badge.style.display='none';
  }
}

// ── AUTO-REFRESH (lecture seule depuis Sheets, ne remplace jamais le local) ──
async function autoRefresh(){
  // ✅ FIX BUG 2 : ne pas rafraîchir si une sauvegarde est en cours
  if(isSaving||pendingSave)return;

  // S'il y a des saves en attente depuis la dernière fois → réessayer
  try{
    const hasPending=localStorage.getItem(LS_PENDING_KEY);
    if(hasPending){
      localStorage.removeItem(LS_PENDING_KEY);
      console.log('🔄 Retry sauvegarde en attente...');
      await pushToSheets();
      return;
    }
  }catch(e){}

  // Rafraîchissement doux : ne remplace que si Sheets a PLUS de données
  try{
    const ctrl=new AbortController();
    setTimeout(()=>ctrl.abort(),6000);
    const res=await fetch(SCRIPT_URL+'?t='+Date.now(),{
      method:'GET',mode:'cors',cache:'no-cache',
      headers:{'Accept':'application/json'},
      signal:ctrl.signal
    });
    if(!res.ok)return;
    const raw=await res.json();
    if(!Array.isArray(raw)||raw.length===0)return;
    const remote=raw.map(parseRow).filter(p=>p.nom).map(enrichProspect);

    // Ne mettre à jour que si Sheets a des données fraîches et plus complètes
    if(remote.length>=prospects.length&&remote.length>0){
      // Comparer les IDs pour voir s'il y a de vraies différences
      const remoteIds=new Set(remote.map(p=>p.id));
      const localIds=new Set(prospects.map(p=>p.id));
      const hasNew=[...remoteIds].some(id=>!localIds.has(id));
      if(hasNew){
        prospects=remote;
        mergeMissingProspects(false);
        saveLocalNow();
        renderAll();
        if(detailOpen){
          const dv=document.getElementById('detail-view');
          const pid=dv?.dataset?.pid;
          if(pid&&prospects.find(p=>p.id===pid))openDetailView(pid);
        }
        setSyncStatus(`✅ Synchronisé (${prospects.length})`,'#059669',false);
      }
    }
  }catch(e){
    // Silencieux — l'auto-refresh est une fonctionnalité secondaire
  }
}

function parseRow(row){
  let pa=null,ae=[];
  try{pa=row.prochaineAction?JSON.parse(row.prochaineAction):null;}catch(e){}
  try{ae=row.actionsEffectuees?JSON.parse(row.actionsEffectuees):[];}catch(e){}
  return{
    id:row.id||genId(),
    nom:row.nom||'',
    contactNom:row.contactNom||'',
    contactTel:String(row.contactTel||''),
    contactEmail:row.contactEmail||'',
    adresse:row.adresse||'',
    siret:String(row.siret||''),
    categorie:row.categorie||'chaud',
    formation:row.formation||'',
    commentaire:row.commentaire||'',
    prochaineAction:pa,
    actionsEffectuees:Array.isArray(ae)?ae:[],
    createdAt:row.createdAt||new Date().toISOString()
  };
}

function genId(){return Date.now().toString(36)+Math.random().toString(36).substr(2);}

// ── FORMAT TEL ──
function formatTel(raw){
  if(raw===null||raw===undefined)return'';
  raw=String(raw).trim();
  if(!raw||raw==='-'||raw==='—'||raw==='0')return raw;
  if(raw.includes('/')||raw.includes('|'))return raw.split(/[\/|]/).map(p=>formatTel(p.trim())).join(' / ');
  let d=raw.replace(/\D/g,'');
  if(!d)return raw;
  if(d.startsWith('33')&&d.length===11)d='0'+d.slice(2);
  if(d.length===9)d='0'+d;
  if(d.length===10)return d.replace(/(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/,'$1 $2 $3 $4 $5');
  return raw;
}

// ── SLOTS ──
const SLOTS=['09:00','09:30','10:00','10:30','11:00','11:30','14:00','14:30','15:00','15:30','16:00','16:30','17:00','17:30'];
function deterministicSlot(seed){
  const h=Math.abs((seed||'x').split('').reduce((a,c)=>a+c.charCodeAt(0),0))%SLOTS.length;
  return{hstart:SLOTS[h],hend:SLOTS[Math.min(h+1,SLOTS.length-1)]};
}

function enrichProspect(p){
  p.contactTel=formatTel(p.contactTel);
  if(p.prochaineAction&&!p.prochaineAction.hstart){
    const s=deterministicSlot(p.id||p.nom);
    p.prochaineAction.hstart=s.hstart;
    p.prochaineAction.hend=s.hend;
  }
  return p;
}

// ── URGENCY ──
function today0(){const d=new Date();d.setHours(0,0,0,0);return d;}
function getUrgency(action){
  if(!action)return{cls:'none',lbl:'',ico:''};
  const now=today0(),d=new Date(action.date);d.setHours(0,0,0,0);
  const diff=(d-now)/86400000;
  if(diff<0)return{cls:'overdue',lbl:'EN RETARD',ico:'🔴'};
  if(diff===0)return{cls:'today',lbl:"AUJOURD'HUI",ico:'🟡'};
  if(diff<=3)return{cls:'soon',lbl:'BIENTÔT',ico:'🔵'};
  return{cls:'normal',lbl:'À PLANIFIER',ico:'📌'};
}

// ── FORMAT ──
function esc(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function fmtDate(ds){if(!ds)return'—';return new Date(ds).toLocaleDateString('fr-FR',{day:'2-digit',month:'2-digit',year:'numeric'});}
function fmtDT(ds){if(!ds)return'—';return new Date(ds).toLocaleString('fr-FR',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});}
function fmtDayFull(d){return DAYS_FR[d.getDay()]+' '+d.getDate()+' '+MONTHS_FR[d.getMonth()]+' '+d.getFullYear();}

// ── RENDER ALL ──
function renderAll(){
  renderStats();
  renderSidebarCounts();
  renderProspects();
  updateDeletedCount();
  if(currentView==='agenda')renderAgenda();
}

// ── STATS ──
function renderStats(){
  const now=today0(),we=new Date(now);we.setDate(we.getDate()+7);
  document.getElementById('stat-total').textContent=prospects.length;
  document.getElementById('stat-retard').textContent=prospects.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now).length;
  document.getElementById('stat-today').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();}).length;
  document.getElementById('stat-nego').textContent=prospects.filter(p=>p.categorie==='negociation').length;
  document.getElementById('stat-orga').textContent=prospects.filter(p=>p.categorie==='organisation').length;
}

function renderSidebarCounts(){
  const now=today0(),we=new Date(now);we.setDate(we.getDate()+7);
  Object.keys(CAT).forEach(c=>{const el=document.getElementById('cnt-'+c);if(el)el.textContent=prospects.filter(p=>p.categorie===c).length;});
  document.getElementById('cnt-overdue').textContent=prospects.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now).length;
  document.getElementById('cnt-today').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();}).length;
  document.getElementById('cnt-week').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);return d>=now&&d<=we;}).length;
  document.getElementById('sb-agenda-count').textContent=prospects.filter(p=>p.prochaineAction).length;
  ['all','organisation','negociation','chaud','autre','gagne'].forEach(f=>{const el=document.getElementById('fc-'+f);if(el)el.textContent=f==='all'?prospects.length:prospects.filter(p=>p.categorie===f).length;});
}

// ── FILTERS ──
function setFilter(f){
  currentFilter=f;filterMode='cat';
  document.querySelectorAll('.ftab').forEach(t=>t.classList.toggle('active',t.dataset.f===f));
  renderProspects();
}

function applyFilter(f){
  showView('dashboard');
  if(['overdue','today','week'].includes(f)){
    filterMode='urgency';currentFilter=f;
    document.querySelectorAll('.ftab').forEach(t=>t.classList.remove('active'));
  }else{
    filterMode='cat';currentFilter=f;
    document.querySelectorAll('.ftab').forEach(t=>t.classList.toggle('active',t.dataset.f===f));
  }
  // Highlight the matching chip
  const chipMap={all:'chip-total',overdue:'chip-overdue',today:'chip-today',negociation:'chip-nego',organisation:'chip-orga'};
  document.querySelectorAll('.tb-chip').forEach(c=>c.classList.remove('active'));
  if(chipMap[f])document.getElementById(chipMap[f])?.classList.add('active');
  renderProspects();
}

function handleSearch(v){
  searchQuery=v;
  if(v)filterMode='cat'; // reset urgency filter when searching
  renderProspects();
}

function clearSearch(){
  document.getElementById('search-input').value='';
  searchQuery='';
  renderProspects();
}

function getFiltered(){
  let list=[...prospects];
  const now=today0(),we=new Date(now);we.setDate(we.getDate()+7);
  if(filterMode==='urgency'){
    if(currentFilter==='overdue')list=list.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now);
    else if(currentFilter==='today')list=list.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();});
    else if(currentFilter==='week')list=list.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);return d>=now&&d<=we;});
  }else if(currentFilter!=='all'){
    list=list.filter(p=>p.categorie===currentFilter);
  }
  if(searchQuery){
    const q=searchQuery.toLowerCase();
    list=list.filter(p=>[p.nom,p.contactNom,p.contactEmail,p.contactTel,p.adresse,p.formation,p.commentaire,p.siret,p.prochaineAction?.description].some(v=>(v||'').toString().toLowerCase().includes(q)));
  }
  const sort=document.getElementById('sort-select')?.value||'urgency';
  if(sort==='urgency')list.sort((a,b)=>{const sc=p=>{if(!p.prochaineAction)return 9999;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return(d-now)/86400000;};return sc(a)-sc(b);});
  else if(sort==='name')list.sort((a,b)=>a.nom.localeCompare(b.nom,'fr'));
  else if(sort==='date-asc')list.sort((a,b)=>{const da=a.prochaineAction?new Date(a.prochaineAction.date):new Date(9999,0);const db=b.prochaineAction?new Date(b.prochaineAction.date):new Date(9999,0);return da-db;});
  else if(sort==='date-desc')list.sort((a,b)=>{const da=a.prochaineAction?new Date(a.prochaineAction.date):new Date(0);const db=b.prochaineAction?new Date(b.prochaineAction.date):new Date(0);return db-da;});
  else if(sort==='recent')list.sort((a,b)=>new Date(b.createdAt)-new Date(a.createdAt));
  else if(sort==='hist')list.sort((a,b)=>(b.actionsEffectuees?.length||0)-(a.actionsEffectuees?.length||0));
  return list;
}

function setViewMode(m){
  viewMode=m;
  document.getElementById('vt-grid').classList.toggle('active',m==='grid');
  document.getElementById('vt-list').classList.toggle('active',m==='list');
  renderProspects();
}

// ── RENDER PROSPECTS ──
function renderProspects(){
  const container=document.getElementById('prospects-container');
  const empty=document.getElementById('empty-state');
  const filtered=getFiltered();
  if(!filtered.length){container.innerHTML='';empty.style.display='block';return;}
  empty.style.display='none';
  container.innerHTML=viewMode==='list'
    ?'<div class="prospects-list">'+filtered.map(renderListRow).join('')+'</div>'
    :'<div class="prospects-grid">'+filtered.map(renderCard).join('')+'</div>';
}

function scoreProspect(p){
  // 0-5 score based on completeness and engagement
  let s=0;
  if(p.contactEmail)s++;
  if(p.contactTel&&p.contactTel!=='-')s++;
  if(p.formation)s++;
  if(p.prochaineAction)s++;
  if(p.actionsEffectuees?.length>0)s++;
  return s;
}

function renderScoreDots(p){
  const s=scoreProspect(p);
  const col=s>=4?'green':s>=2?'orange':'red';
  return`<div class="score-ring" title="Score dossier ${s}/5"><div class="score-dots">${Array.from({length:5},(_,i)=>`<div class="sdot${i<s?' on '+col:''}"></div>`).join('')}</div></div>`;
}

// ── CARD ──
function renderCard(p){
  const cat=CAT[p.categorie]||CAT.autre;
  const urg=getUrgency(p.prochaineAction);
  const hc=p.actionsEffectuees?.length||0;
  return`<div class="card" onclick="openDetailView('${p.id}')">
<div class="card-urgency-bar ${urg.cls}"></div>
<div class="card-hdr">
  <div style="flex:1;min-width:0">
    <div class="card-name">${esc(p.nom)}</div>
    <div class="card-badges">
      <span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span>
      ${p.formation?`<span style="font-size:10px;background:var(--teal-l);color:var(--teal);padding:2px 8px;border-radius:12px;font-weight:700">🎓 ${esc(p.formation)}</span>`:''}
      ${hc>0?`<span class="score-badge">📜 ${hc} action${hc>1?'s':''}</span>`:''}
    </div>
  </div>
  <div class="card-acts readonly-hide" onclick="event.stopPropagation()">
    <button class="ibt" onclick="editProspect('${p.id}')" title="Modifier">✏️</button>
    <button class="ibt del" onclick="deleteProspect('${p.id}')" title="Supprimer">🗑️</button>
  </div>
</div>
<div class="card-body">
  <div class="ci-grid">
    ${p.contactNom?`<div class="ci"><div class="ci-ico">👤</div><span title="${esc(p.contactNom)}">${esc(p.contactNom)}</span></div>`:''}
    ${p.contactTel&&p.contactTel!=='-'?`<div class="ci"><div class="ci-ico">📞</div><span>${esc(p.contactTel)}</span></div>`:''}
    ${p.contactEmail?`<div class="ci"><div class="ci-ico">✉️</div><span title="${esc(p.contactEmail)}">${esc(p.contactEmail)}</span></div>`:''}
    ${p.adresse?`<div class="ci"><div class="ci-ico">📍</div><span>${esc(p.adresse)}</span></div>`:''}
  </div>
  ${p.commentaire?`<div class="cnotes">${esc(p.commentaire)}</div>`:''}
  ${p.prochaineAction?`
  <div class="abox ${urg.cls}">
    <div class="abox-hdr">
      <span class="aurge">${urg.ico} ${urg.lbl}</span>
      <div style="text-align:right">
        <div class="adate">${fmtDate(p.prochaineAction.date)}</div>
        ${p.prochaineAction.hstart?`<div class="atime">⏰ ${p.prochaineAction.hstart}${p.prochaineAction.hend?' – '+p.prochaineAction.hend:''}</div>`:''}
      </div>
    </div>
    <div class="adesc">${esc(p.prochaineAction.description)}</div>
    <button class="acpl" onclick="event.stopPropagation();completeAction('${p.id}')">✓ Marquer effectuée</button>
  </div>`:`
  <div class="abox none">
    <button class="add-action" onclick="event.stopPropagation();openActionModal('${p.id}')">+ Planifier une action</button>
  </div>`}
</div></div>`;
}

// ── LIST ROW ──
function renderListRow(p){
  const cat=CAT[p.categorie]||CAT.autre;
  const urg=getUrgency(p.prochaineAction);
  return`<div class="list-row urg-${urg.cls}" onclick="openDetailView('${p.id}')">
<div class="lr-name">${esc(p.nom)}<br><span style="font-size:11px;color:var(--txt3);font-weight:400">${p.adresse||''}</span></div>
<div><span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span></div>
<div style="font-size:12.5px;color:var(--txt2)">${p.contactNom?esc(p.contactNom):'—'}<br><span style="font-size:11.5px;font-weight:600">${p.contactTel&&p.contactTel!=='-'?esc(p.contactTel):''}</span></div>
<div style="font-size:12px;color:var(--txt2)">${p.prochaineAction?esc(p.prochaineAction.description):'<em style="color:var(--txt3)">Aucune action</em>'}</div>
<div>${p.prochaineAction?`<span style="font-size:12px;font-weight:700;color:${urg.cls==='overdue'?'var(--red)':urg.cls==='today'?'var(--orange)':'var(--blue)'}">${urg.ico} ${fmtDate(p.prochaineAction.date)}</span>`:'—'}</div>
<div class="card-acts readonly-hide" onclick="event.stopPropagation()">
  <button class="ibt" onclick="editProspect('${p.id}')" title="Modifier">✏️</button>
  ${p.prochaineAction?`<button class="ibt" onclick="completeAction('${p.id}')" title="Effectuée" style="color:var(--green)">✅</button>`:''}
  <button class="ibt del" onclick="deleteProspect('${p.id}')">🗑️</button>
</div></div>`;
}

// ── VIEWS ──
function showView(v){
  currentView=v;
  document.querySelectorAll('.view').forEach(el=>el.classList.remove('active'));
  document.getElementById('view-'+v).classList.add('active');
  document.querySelectorAll('.ni').forEach(n=>n.classList.remove('active'));
  document.getElementById('ni-'+v)?.classList.add('active');
  if(v==='agenda')renderAgenda();
  if(v==='dashboard'){closeDV();}
}

// ── AGENDA ──
function agNav(dir){if(agView==='day')agDayOffset+=dir;else agOffset+=dir;renderAgenda();}
function agToday(){agOffset=0;agDayOffset=0;renderAgenda();}
function setAgView(v){
  agView=v;
  ['list','week','day'].forEach(x=>document.getElementById('ag-'+x)?.classList.toggle('active',x===v));
  renderAgenda();
}
function getWeekStart(off){
  const d=new Date();const day=d.getDay();const diff=d.getDate()-(day===0?6:day-1)+off*7;
  d.setDate(diff);d.setHours(0,0,0,0);return d;
}
function getDayActions(date){
  const d0=new Date(date);d0.setHours(0,0,0,0);
  return prospects.filter(p=>{
    if(!p.prochaineAction)return false;
    const pd=new Date(p.prochaineAction.date);pd.setHours(0,0,0,0);
    return pd.getTime()===d0.getTime();
  }).map(p=>({...p.prochaineAction,pid:p.id,nom:p.nom,categorie:p.categorie}));
}
function renderAgenda(){
  const c=document.getElementById('agenda-content');if(!c)return;
  if(agView==='week')renderWeek(c);
  else if(agView==='day')renderDay(c);
  else renderAgendaList(c);
}

function renderWeek(c){
  const ws=getWeekStart(agOffset);const we=new Date(ws);we.setDate(we.getDate()+6);
  document.getElementById('ag-label').textContent=`Semaine du ${ws.getDate()} au ${we.getDate()} ${MONTHS_FR[we.getMonth()]} ${we.getFullYear()}`;
  const days=Array.from({length:7},(_,i)=>{const d=new Date(ws);d.setDate(ws.getDate()+i);return d;});
  const ts=today0().toDateString();
  let html='<div class="week-grid"><div class="wg-corner" style="padding:10px;background:var(--bg3);border-bottom:1px solid var(--bl);border-right:1px solid var(--bl)"></div>';
  days.forEach(d=>{
    const iT=d.toDateString()===ts;
    html+=`<div class="wg-dayhdr${iT?' today':''}"><div class="wg-dayname">${DAYS_FR[d.getDay()]}</div><div class="wg-daynum">${d.getDate()}</div></div>`;
  });
  HOURS.forEach(h=>{
    const hn=parseInt(h);
    html+=`<div class="wg-time">${h}</div>`;
    days.forEach(d=>{
      const iT=d.toDateString()===ts;
      const acts=getDayActions(d).filter(a=>a.hstart&&parseInt(a.hstart)===hn);
      html+=`<div class="wg-cell${iT?' today':''}">`;
      acts.forEach(a=>{
        const urg=getUrgency({date:a.date});
        html+=`<div class="wg-event ${urg.cls}" onclick="openDetailView('${a.pid}')" title="${esc(a.nom)} — ${esc(a.description)}">${esc(a.nom)}</div>`;
      });
      if(!acts.length)html+=`<div class="wg-add" onclick="quickAdd('${d.toISOString().split('T')[0]}','${h}')">+</div>`;
      html+='</div>';
    });
  });
  html+='</div>';
  c.innerHTML=`<div style="overflow-x:auto;padding:0 0 24px">${html}</div>`;
}

function renderDay(c){
  const d=new Date();d.setDate(d.getDate()+agDayOffset);d.setHours(0,0,0,0);
  const iT=d.toDateString()===today0().toDateString();
  document.getElementById('ag-label').textContent=`${DAYS_FR[d.getDay()]} ${d.getDate()} ${MONTHS_FR[d.getMonth()]} ${d.getFullYear()}${iT?' — Aujourd\'hui':''}`;
  const acts=getDayActions(d);
  const noTime=acts.filter(a=>!a.hstart);
  let html='<div class="day-grid">';
  HOURS.forEach((h,hi)=>{
    const hn=parseInt(h);
    const slotActs=acts.filter(a=>a.hstart&&parseInt(a.hstart)===hn);
    html+=`<div class="day-time">${h}</div><div class="day-slot">`;
    if(hi===0&&noTime.length)noTime.forEach(a=>{
      const urg=getUrgency({date:a.date});
      html+=`<div class="day-event ${urg.cls}" onclick="openDetailView('${a.pid}')"><span style="font-size:10px;opacity:.7">🕐 libre</span><span style="flex:1">${esc(a.nom)} — ${esc(a.description)}</span><button class="cpl-mini" onclick="event.stopPropagation();completeAction('${a.pid}')">✓</button></div>`;
    });
    slotActs.forEach(a=>{
      const urg=getUrgency({date:a.date});
      html+=`<div class="day-event ${urg.cls}" onclick="openDetailView('${a.pid}')"><span style="font-size:11px;font-weight:700">${a.hstart}${a.hend?' – '+a.hend:''}</span><span style="flex:1">${esc(a.nom)} — ${esc(a.description)}</span><button class="cpl-mini" onclick="event.stopPropagation();completeAction('${a.pid}')">✓</button></div>`;
    });
    if(!slotActs.length&&!(hi===0&&noTime.length))html+=`<div class="wg-add" onclick="quickAdd('${d.toISOString().split('T')[0]}','${h}')">+</div>`;
    html+='</div>';
  });
  html+='</div>';
  c.innerHTML=`<div style="overflow-x:auto">${html}</div>`;
}

function renderAgendaList(c){
  const now=today0();
  const all=prospects.filter(p=>p.prochaineAction).sort((a,b)=>new Date(a.prochaineAction.date)-new Date(b.prochaineAction.date));
  if(!all.length){c.innerHTML='<div style="text-align:center;padding:80px;color:var(--txt3)"><div style="font-size:48px;margin-bottom:16px">📅</div><div style="font-size:18px;font-weight:700">Aucune action planifiée</div></div>';return;}
  const groups={};
  all.forEach(p=>{const dk=p.prochaineAction.date;if(!groups[dk])groups[dk]=[];groups[dk].push(p);});
  let html='<div class="ag-list-wrap">';
  Object.keys(groups).sort().forEach(dk=>{
    const d=new Date(dk);const isT=d.toDateString()===now.toDateString();const isP=d<now;
    const col=isP?'var(--red)':isT?'var(--orange)':'var(--txt)';
    html+=`<div class="ag-group"><div class="ag-group-title" style="color:${col}">📅 ${fmtDayFull(d)} ${isT?`<span class="ag-tag" style="background:var(--orange)">AUJOURD'HUI</span>`:''}${isP?`<span class="ag-tag" style="background:var(--red)">EN RETARD</span>`:''}</div>`;
    groups[dk].sort((a,b)=>(a.prochaineAction.hstart||'99')>(b.prochaineAction.hstart||'99')?1:-1).forEach(p=>{
      const a=p.prochaineAction;const urg=getUrgency(a);const cat=CAT[p.categorie]||CAT.autre;
      html+=`<div class="ag-item ${urg.cls}" onclick="openDetailView('${p.id}')">
<div class="ag-time">${a.hstart||'—'}</div>
<div class="ag-dot" style="background:${cat.c}"></div>
<div style="flex:1;min-width:0">
  <div class="ag-prospect">${esc(p.nom)}</div>
  <div class="ag-action">${esc(a.description)}${p.contactTel&&p.contactTel!=='-'?' · 📞 '+esc(p.contactTel):''}</div>
</div>
<span class="cbadge ${p.categorie}">${cat.i}</span>
<button class="ag-cpl" onclick="event.stopPropagation();completeAction('${p.id}')">✓ Effectuée</button>
</div>`;
    });
    html+='</div>';
  });
  html+='</div>';
  c.innerHTML=html;
}

function quickAdd(ds,hs){
  openActionModal(null,true,ds,hs);
}

// ── DETAIL VIEW ──
function openDetailView(id){
  const p=prospects.find(x=>x.id===id);if(!p)return;
  // S'assurer qu'on est dans le dashboard
  if(currentView!=='dashboard'){showView('dashboard');}
  const cat=CAT[p.categorie]||CAT.autre;
  const urg=getUrgency(p.prochaineAction);
  const hc=p.actionsEffectuees?.length||0;
  const dv=document.getElementById('detail-view');
  dv.dataset.pid=id;
  dv.innerHTML=`
<div class="dv-hdr">
  <div class="dv-hdr-top">
    <button class="dv-back" onclick="closeDV()">← Retour à la liste</button>
    <div class="hdr-actions">
      <button class="btn btn-secondary btn-sm" onclick="editProspect('${p.id}')">✏️ Modifier</button>
      <button class="btn btn-danger btn-sm" onclick="deleteProspect('${p.id}')">🗑️ Supprimer</button>
    </div>
  </div>
  <div class="dv-meta"><span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span>${p.formation?`<span style="background:rgba(255,255,255,.15);color:#fff;padding:3px 10px;border-radius:12px;font-size:11px;font-weight:700">🎓 ${esc(p.formation)}</span>`:''}${hc?`<span style="background:rgba(255,255,255,.12);color:rgba(255,255,255,.8);padding:3px 10px;border-radius:12px;font-size:11px">📜 ${hc} action${hc>1?'s':''}</span>`:''}</div>
  <div class="dv-name">${esc(p.nom)}</div>
  <div class="dv-info-grid">
    <div class="dv-info-item"><div class="dv-info-lbl">Contact</div><div class="dv-info-val">${esc(p.contactNom||'—')}</div></div>
    <div class="dv-info-item"><div class="dv-info-lbl">Téléphone</div><div class="dv-info-val">${p.contactTel&&p.contactTel!=='-'?`<a href="tel:${esc(p.contactTel)}">${esc(p.contactTel)}</a>`:esc(p.contactTel||'—')}</div></div>
    <div class="dv-info-item"><div class="dv-info-lbl">Email</div><div class="dv-info-val" style="font-size:12px">${p.contactEmail?`<a href="mailto:${esc(p.contactEmail)}">${esc(p.contactEmail)}</a>`:'—'}</div></div>
    <div class="dv-info-item"><div class="dv-info-lbl">Adresse</div><div class="dv-info-val">${esc(p.adresse||'—')}</div></div>
  </div>
</div>
<div class="dv-body">
  <div class="dv-main">
    ${p.commentaire?`<div class="panel"><div class="panel-hdr">📝 Notes & Contexte</div><div class="panel-body" style="line-height:1.7;color:var(--txt2);white-space:pre-wrap">${esc(p.commentaire)}</div></div>`:''}
    <div class="panel">
      <div class="panel-hdr">
        <span>📜 Historique des actions (${hc})</span>
        <button class="btn btn-secondary btn-xs" onclick="openActionModal('${p.id}')">+ Planifier</button>
      </div>
      <div class="panel-body">
        ${hc>0?`<div class="timeline">${p.actionsEffectuees.slice().reverse().map(a=>`
<div class="tl-item">
  <div class="tl-dot">✓</div>
  <div class="tl-content">
    <div class="tl-date">${fmtDT(a.completedAt)}</div>
    <div class="tl-action">${esc(a.description)}</div>
    ${a.commentaire?`<div class="tl-comment">💬 ${esc(a.commentaire)}</div>`:''}
  </div>
</div>`).join('')}</div>`
:'<div style="text-align:center;padding:28px;color:var(--txt3)">Aucune action effectuée — commencez à tracer votre suivi !</div>'}
      </div>
    </div>
  </div>
  <div class="dv-aside">
    <div class="action-panel">
      <div class="ap-hdr"><h3>🎯 Prochaine action</h3><p>Suivi commercial du dossier</p></div>
      <div class="ap-body">
        ${p.prochaineAction?`
<div class="cur-action ${urg.cls}">
  <div class="ca-lbl">${urg.ico} ${urg.lbl}</div>
  <div class="ca-text">${esc(p.prochaineAction.description)}</div>
  <div class="ca-date">📅 ${fmtDate(p.prochaineAction.date)}</div>
  ${p.prochaineAction.hstart?`<div class="ca-time">⏰ ${p.prochaineAction.hstart}${p.prochaineAction.hend?' – '+p.prochaineAction.hend:''}</div>`:''}
  ${p.prochaineAction.assignee?`<div style="font-size:11.5px;color:var(--txt3);margin-top:4px">👤 ${esc(p.prochaineAction.assignee)}</div>`:''}
</div>
<div class="qa-list">
  <button class="qa-btn complete" onclick="completeAction('${p.id}')">✓ Marquer comme effectuée</button>
  <button class="qa-btn secondary" onclick="openActionModal('${p.id}')">📅 Modifier / Reprogrammer</button>
</div>`:`
<div style="text-align:center;padding:22px 0">
  <div style="font-size:32px;margin-bottom:10px">📭</div>
  <p style="color:var(--txt3);margin-bottom:14px;font-size:13px">Aucune action planifiée</p>
  <button class="btn btn-primary btn-sm" onclick="openActionModal('${p.id}')">+ Planifier une action</button>
</div>`}
      </div>
    </div>
    <div class="panel">
      <div class="panel-hdr">⚡ Changer le statut</div>
      <div class="panel-body">
        <div class="cat-grid">
          ${Object.entries(CAT).map(([k,v])=>`<div class="cat-choice${p.categorie===k?' current':''}" onclick="changeCat('${p.id}','${k}')"><span style="width:8px;height:8px;border-radius:50%;background:${v.c};flex-shrink:0;display:inline-block"></span>${v.i} ${v.l}</div>`).join('')}
        </div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-hdr">ℹ️ Informations</div>
      <div class="panel-body" style="display:grid;grid-template-columns:1fr 1fr;gap:12px">
        <div><div style="font-size:10px;color:var(--txt3);text-transform:uppercase;font-weight:700;margin-bottom:3px">SIRET</div><div style="font-size:12.5px;font-weight:600">${esc(p.siret||'—')}</div></div>
        <div><div style="font-size:10px;color:var(--txt3);text-transform:uppercase;font-weight:700;margin-bottom:3px">Créé le</div><div style="font-size:12.5px;font-weight:600">${fmtDate(p.createdAt)}</div></div>
        <div><div style="font-size:10px;color:var(--txt3);text-transform:uppercase;font-weight:700;margin-bottom:3px">Score dossier</div><div>${renderScoreDots(p)}</div></div>
        <div><div style="font-size:10px;color:var(--txt3);text-transform:uppercase;font-weight:700;margin-bottom:3px">Actions</div><div style="font-size:12.5px;font-weight:600">${hc} effectuée${hc>1?'s':''}</div></div>
      </div>
    </div>
  </div>
</div>`;

  document.getElementById('dash-content').style.display='none';
  dv.classList.add('open');
  detailOpen=true;
}

function closeDV(){
  document.getElementById('detail-view').classList.remove('open');
  document.getElementById('dash-content').style.display='';
  detailOpen=false;
}

function changeCat(id,cat){
  const i=prospects.findIndex(p=>p.id===id);
  if(i!==-1){prospects[i].categorie=cat;saveData();openDetailView(id);showNotif('Statut mis à jour → '+CAT[cat].l);}
}

// ── CRUD ──
function openProspectModal(id=null){
  document.getElementById('modal-title').textContent=id?'Modifier le prospect':'Nouveau prospect';
  if(id){
    const p=prospects.find(x=>x.id===id);if(!p)return;
    document.getElementById('prospect-id').value=p.id;
    document.getElementById('f-nom').value=p.nom||'';
    document.getElementById('f-siret').value=p.siret||'';
    document.getElementById('f-adresse').value=p.adresse||'';
    document.getElementById('f-contact-nom').value=p.contactNom||'';
    document.getElementById('f-contact-tel').value=p.contactTel||'';
    document.getElementById('f-contact-email').value=p.contactEmail||'';
    document.getElementById('f-formation').value=p.formation||'';
    document.getElementById('f-commentaire').value=p.commentaire||'';
    selectCat(p.categorie||'chaud');
    if(p.prochaineAction){
      document.getElementById('f-action-desc').value=p.prochaineAction.description||'';
      document.getElementById('f-action-date').value=p.prochaineAction.date||'';
      document.getElementById('f-action-hstart').value=p.prochaineAction.hstart||'';
      document.getElementById('f-action-hend').value=p.prochaineAction.hend||'';
      document.getElementById('f-action-assignee').value=p.prochaineAction.assignee||'';
    }
  }else{
    document.getElementById('prospect-form').reset();
    document.getElementById('prospect-id').value='';
    selectCat('chaud');
    // Suggest today's date
    document.getElementById('f-action-date').value=new Date().toISOString().split('T')[0];
  }
  document.getElementById('prospect-modal').classList.add('open');
}
function closeProspectModal(){document.getElementById('prospect-modal').classList.remove('open');}

function handleProspectSubmit(e){
  e.preventDefault();
  const id=document.getElementById('prospect-id').value;
  const data={
    nom:document.getElementById('f-nom').value.trim(),
    siret:document.getElementById('f-siret').value.trim(),
    adresse:document.getElementById('f-adresse').value.trim(),
    contactNom:document.getElementById('f-contact-nom').value.trim(),
    contactTel:formatTel(document.getElementById('f-contact-tel').value.trim()),
    contactEmail:document.getElementById('f-contact-email').value.trim(),
    categorie:selectedCat,
    formation:document.getElementById('f-formation').value.trim(),
    commentaire:document.getElementById('f-commentaire').value.trim()
  };
  const adesc=document.getElementById('f-action-desc').value.trim();
  const adate=document.getElementById('f-action-date').value;
  const pa=adesc&&adate?{description:adesc,date:adate,hstart:document.getElementById('f-action-hstart').value,hend:document.getElementById('f-action-hend').value,assignee:document.getElementById('f-action-assignee').value,createdAt:new Date().toISOString()}:null;

  if(id){
    const i=prospects.findIndex(p=>p.id===id);
    if(i!==-1){
      prospects[i]={...prospects[i],...data};
      if(pa)prospects[i].prochaineAction=pa;
      showNotif('Prospect mis à jour ✓');
    }
  }else{
    const np={id:genId(),...data,actionsEffectuees:[],prochaineAction:pa,createdAt:new Date().toISOString()};
    prospects.push(np);
    showNotif('Nouveau prospect créé ✓');
  }
  saveData();closeProspectModal();renderAll();
}

function editProspect(id){closeProspectModal();openProspectModal(id);}

function deleteProspect(id){
  const p=prospects.find(x=>x.id===id);
  if(!p)return;
  if(!confirm(`Supprimer "${p.nom}" ?\n\n⚠️ Cette action sera tracée dans l'historique des suppressions.`))return;
  
  // 🗑️ TRAÇABILITÉ : Enregistrer la suppression
  const deletionRecord={
    ...p,
    deletedAt:new Date().toISOString(),
    deletedBy:'Utilisateur', // À personnaliser si multi-utilisateurs
    reason:'Suppression manuelle'
  };
  deletedProspects.push(deletionRecord);
  
  // Sauvegarder l'historique des suppressions dans localStorage
  try{
    localStorage.setItem('siphro_deleted_prospects',JSON.stringify(deletedProspects));
  }catch(e){console.warn('Impossible de sauvegarder les suppressions:',e);}
  
  prospects=prospects.filter(p=>p.id!==id);
  saveData();closeDV();renderAll();
  showNotif(`"${p.nom}" supprimé et archivé`,'warning');
  console.log('🗑️ Prospect supprimé:',deletionRecord);
}

function selectCat(c){
  selectedCat=c;
  document.querySelectorAll('.cat-opt').forEach(o=>o.classList.toggle('selected',o.dataset.v===c));
}
function selAtype(btn){
  document.querySelectorAll('#f-atype-sel .atype').forEach(b=>b.classList.remove('selected'));
  btn.classList.add('selected');
  // Auto-fill description if empty
  const desc=document.getElementById('f-action-desc');
  if(!desc.value)desc.value=btn.dataset.v+' — ';
  desc.focus();
}
function selAtypeAction(btn){
  document.querySelectorAll('#action-atype-sel .atype').forEach(b=>b.classList.remove('selected'));
  btn.classList.add('selected');
  const desc=document.getElementById('action-desc');
  if(!desc.value)desc.value=btn.dataset.v+' — ';
  desc.focus();
}

// ── ACTION MODAL ──
function openActionModal(pid=null,fromAgenda=false,preDate='',preHour=''){
  document.getElementById('action-form').reset();
  const ps=document.getElementById('action-prospect-search');
  const pidEl=document.getElementById('action-prospect-id');
  if(pid){const p=prospects.find(x=>x.id===pid);if(p){ps.value=p.nom;pidEl.value=pid;}}
  else{ps.value='';pidEl.value='';}
  const today=new Date().toISOString().split('T')[0];
  document.getElementById('action-date').value=preDate||today;
  if(preHour)document.getElementById('action-hstart').value=preHour;
  document.getElementById('action-modal-title').textContent=fromAgenda?'Nouvelle tâche agenda':'Planifier une action';
  document.getElementById('action-modal').classList.add('open');
}
function closeActionModal(){
  document.getElementById('action-modal').classList.remove('open');
  document.getElementById('prospect-dropdown').style.display='none';
}
function searchProspectDD(v){
  const dd=document.getElementById('prospect-dropdown');
  if(!v||v.length<1){dd.style.display='none';return;}
  const matches=prospects.filter(p=>p.nom.toLowerCase().includes(v.toLowerCase())||p.contactNom.toLowerCase().includes(v.toLowerCase())).slice(0,8);
  if(!matches.length){dd.style.display='none';return;}
  dd.innerHTML=matches.map(p=>`<div class="dd-item" onclick="selectProspectDD('${p.id}','${esc(p.nom)}')"><strong>${esc(p.nom)}</strong><br><small>${p.contactNom||''} ${p.contactTel&&p.contactTel!=='-'?'· '+p.contactTel:''}</small></div>`).join('');
  dd.style.display='block';
}
function selectProspectDD(id,nom){
  document.getElementById('action-prospect-id').value=id;
  document.getElementById('action-prospect-search').value=nom;
  document.getElementById('prospect-dropdown').style.display='none';
}
function handleActionSubmit(e){
  e.preventDefault();
  const pid=document.getElementById('action-prospect-id').value;
  if(!pid){showNotif('Sélectionnez un prospect','error');return;}
  const desc=document.getElementById('action-desc').value.trim();
  const date=document.getElementById('action-date').value;
  const action={description:desc,date,hstart:document.getElementById('action-hstart').value,hend:document.getElementById('action-hend').value,assignee:document.getElementById('action-assignee').value,notes:document.getElementById('action-notes').value,createdAt:new Date().toISOString()};
  const i=prospects.findIndex(p=>p.id===pid);
  if(i!==-1){
    // Archive old action if exists
    if(prospects[i].prochaineAction){
      if(!prospects[i].actionsEffectuees)prospects[i].actionsEffectuees=[];
      prospects[i].actionsEffectuees.push({...prospects[i].prochaineAction,completedAt:new Date().toISOString(),commentaire:'(Remplacée par nouvelle action)'});
    }
    prospects[i].prochaineAction=action;
    saveData();showNotif('Action planifiée ✅');closeActionModal();
    renderAll();
    if(detailOpen)openDetailView(pid);
  }
}

// ── COMPLETE ACTION ──
function completeAction(pid){
  completePid=pid;
  const p=prospects.find(x=>x.id===pid);
  if(!p||!p.prochaineAction)return;
  document.getElementById('cpl-prospect-name').textContent=p.nom;
  document.getElementById('cpl-action-desc').textContent=p.prochaineAction.description;
  document.getElementById('cpl-comment').value='';
  document.getElementById('cpl-next-desc').value='';
  // Suggest next date = today+7
  const nextDate=new Date();nextDate.setDate(nextDate.getDate()+7);
  document.getElementById('cpl-next-date').value=nextDate.toISOString().split('T')[0];
  document.getElementById('complete-modal').classList.add('open');
}
function closeCompleteModal(){document.getElementById('complete-modal').classList.remove('open');completePid=null;}
function confirmCompleteAction(){
  const pid=completePid;if(!pid)return;
  const i=prospects.findIndex(p=>p.id===pid);
  if(i===-1||!prospects[i].prochaineAction)return;
  const comment=document.getElementById('cpl-comment').value.trim();
  const nextDesc=document.getElementById('cpl-next-desc').value.trim();
  const nextDate=document.getElementById('cpl-next-date').value;
  // Archive
  if(!prospects[i].actionsEffectuees)prospects[i].actionsEffectuees=[];
  prospects[i].actionsEffectuees.push({...prospects[i].prochaineAction,completedAt:new Date().toISOString(),commentaire:comment});
  // Set next action if provided
  if(nextDesc&&nextDate){
    prospects[i].prochaineAction={description:nextDesc,date:nextDate,hstart:'',hend:'',createdAt:new Date().toISOString()};
    enrichProspect(prospects[i]);
  }else{
    prospects[i].prochaineAction=null;
  }
  closeCompleteModal();
  saveData();
  showNotif('Action effectuée ✓ — Historique mis à jour');
  renderAll();
  if(detailOpen)openDetailView(pid);
  if(currentView==='agenda')renderAgenda();
}

// ── IMPORT / EXPORT ──
function openImportModal(){document.getElementById('import-modal').classList.add('open');}
function closeImportModal(){document.getElementById('import-modal').classList.remove('open');}

// ── PROSPECTS SUPPRIMÉS ──
function openDeletedModal(){
  const modal=document.getElementById('deleted-modal');
  const list=document.getElementById('deleted-list');
  
  if(deletedProspects.length===0){
    list.innerHTML='<div style="text-align:center;padding:60px 20px;color:var(--txt2)"><div style="font-size:48px;margin-bottom:16px;opacity:.5">🗑️</div><h3 style="font-size:18px;font-weight:700;margin-bottom:8px">Aucune suppression enregistrée</h3><p style="font-size:13px">L\'historique des suppressions apparaîtra ici</p></div>';
  }else{
    // Trier par date de suppression (plus récent en premier)
    const sorted=[...deletedProspects].sort((a,b)=>new Date(b.deletedAt)-new Date(a.deletedAt));
    
    list.innerHTML=sorted.map(p=>{
      const deleteDate=new Date(p.deletedAt);
      const cat=CAT_LABELS[p.categorie]||CAT_LABELS.autre;
      return`
        <div style="background:var(--bg3);border:1.5px solid var(--bl);border-radius:var(--r3);padding:16px 18px;margin-bottom:12px;border-left:4px solid var(--red)">
          <div style="display:flex;justify-content:space-between;align-items:start;margin-bottom:10px">
            <div>
              <div style="font-size:15px;font-weight:700;margin-bottom:4px">${esc(p.nom)}</div>
              <span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span>
            </div>
            <button class="btn btn-xs btn-secondary" onclick="restoreProspect('${p.id}')" title="Restaurer ce prospect">↩️ Restaurer</button>
          </div>
          <div style="font-size:12px;color:var(--txt2);line-height:1.7">
            ${p.contactNom?`<div><strong>Contact:</strong> ${esc(p.contactNom)}</div>`:''}
            ${p.contactTel?`<div><strong>Tél:</strong> ${esc(p.contactTel)}</div>`:''}
            ${p.formation?`<div><strong>Formation:</strong> ${esc(p.formation)}</div>`:''}
          </div>
          <div style="margin-top:12px;padding-top:12px;border-top:1px solid var(--bl);font-size:11px;color:var(--txt3)">
            🗑️ Supprimé le ${fmtDT(p.deletedAt)} ${p.deletedBy?`par ${esc(p.deletedBy)}`:''}<br>
            ${p.prochaineAction?`⚠️ Action planifiée perdue: ${esc(p.prochaineAction.description)} (${fmtDate(p.prochaineAction.date)})`:''}
            ${p.actionsEffectuees&&p.actionsEffectuees.length>0?`<br>📋 ${p.actionsEffectuees.length} action(s) effectuée(s) archivée(s)`:''}
          </div>
        </div>
      `;
    }).join('');
  }
  
  modal.classList.add('open');
  updateDeletedCount();
}

function closeDeletedModal(){
  document.getElementById('deleted-modal').classList.remove('open');
}

function restoreProspect(id){
  const idx=deletedProspects.findIndex(p=>p.id===id);
  if(idx===-1){showNotif('Prospect introuvable','error');return;}
  
  const restored=deletedProspects[idx];
  // Retirer les métadonnées de suppression
  delete restored.deletedAt;
  delete restored.deletedBy;
  delete restored.reason;
  
  // Réinjecter dans les prospects actifs
  prospects.push(restored);
  deletedProspects.splice(idx,1);
  
  // Sauvegarder
  saveData();
  try{
    localStorage.setItem('siphro_deleted_prospects',JSON.stringify(deletedProspects));
  }catch(e){}
  
  showNotif(`"${restored.nom}" restauré avec succès`,'success');
  renderAll();
  openDeletedModal(); // Rafraîchir la modale
}

function clearDeletedHistory(){
  if(!confirm(`Vider définitivement l'historique des ${deletedProspects.length} suppressions ?\n\n⚠️ Cette action est irréversible.`))return;
  deletedProspects=[];
  try{
    localStorage.removeItem('siphro_deleted_prospects');
  }catch(e){}
  showNotif('Historique vidé','warning');
  closeDeletedModal();
  updateDeletedCount();
}

function updateDeletedCount(){
  const badge=document.getElementById('deleted-count');
  if(badge)badge.textContent=deletedProspects.length;
}
function handleFileSelect(e){
  const file=e.target.files[0];if(!file)return;
  const ext=file.name.split('.').pop().toLowerCase();
  if(ext==='csv')Papa.parse(file,{header:true,delimiter:';',skipEmptyLines:true,complete:r=>importData(r.data)});
  else if(['xlsx','xls'].includes(ext)){const reader=new FileReader();reader.onload=ev=>{const wb=XLSX.read(new Uint8Array(ev.target.result),{type:'array'});importData(XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]));};reader.readAsArrayBuffer(file);}
  e.target.value='';
}
function importData(data){
  let n=0;
  data.forEach(row=>{
    const nom=row.nom_etablissement||row.nom;if(!nom)return;
    const np={id:genId(),nom,contactNom:row.contact_nom||'',contactTel:formatTel(String(row.contact_telephone||'')),contactEmail:row.contact_email||'',adresse:row.adresse||'',siret:row.siret||'',categorie:row.categorie||'chaud',formation:row.formation_visee||'',commentaire:row.commentaire||'',actionsEffectuees:[],prochaineAction:null,createdAt:new Date().toISOString()};
    if(row.prochaine_action&&row.date_prochaine_action)np.prochaineAction={description:row.prochaine_action,date:row.date_prochaine_action,createdAt:new Date().toISOString()};
    prospects.push(enrichProspect(np));n++;
  });
  saveData();closeImportModal();renderAll();showNotif(`${n} prospect(s) importé(s) ✓`);
}
function exportData(){
  // 🔒 EXPORT SÉCURISÉ — Tous les prospects, sans exception
  const allProspects=[...prospects]; // snapshot complet
  if(!allProspects.length){showNotif('Aucun prospect à exporter','warning');return;}
  
  // Génération CSV robuste avec gestion des caractères spéciaux
  function csvCell(v){
    const s=String(v==null?'':v);
    // Si contient virgule, guillemet ou saut de ligne → entourer de guillemets
    if(s.includes(';')||s.includes('"')||s.includes('\n')||s.includes('\r')){
      return'"'+s.replace(/"/g,'""')+'"';
    }
    return s;
  }
  
  const headers=['nom_etablissement','contact_nom','contact_telephone','contact_email','adresse','siret','categorie','formation_visee','commentaire','prochaine_action','date_prochaine_action','heure_debut','heure_fin','nb_actions_effectuees','derniere_action','date_creation','id'];
  
  const rows=allProspects.map(p=>[
    p.nom||'',
    p.contactNom||'',
    p.contactTel||'',
    p.contactEmail||'',
    p.adresse||'',
    p.siret||'',
    p.categorie||'',
    p.formation||'',
    p.commentaire||'',
    p.prochaineAction?.description||'',
    p.prochaineAction?.date||'',
    p.prochaineAction?.hstart||'',
    p.prochaineAction?.hend||'',
    String(p.actionsEffectuees?.length||0),
    p.actionsEffectuees?.length?p.actionsEffectuees[p.actionsEffectuees.length-1].description:'',
    p.createdAt||'',
    p.id||''
  ].map(csvCell).join(';'));
  
  const csv='\ufeff'+headers.join(';')+'\r\n'+rows.join('\r\n');
  const blob=new Blob([csv],{type:'text/csv;charset=utf-8'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=url;
  a.download='siphro-export-'+new Date().toISOString().split('T')[0]+'-'+allProspects.length+'prospects.csv';
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  showNotif(`✅ Export CSV — ${allProspects.length} prospects téléchargés`);
  console.log(`[EXPORT] ${allProspects.length} prospects exportés`);
}

function showBackupInfo(){
  try{
    const bd=localStorage.getItem(LS_KEY+'_date')||localStorage.getItem('siphro_backup_date');
    const bp=loadFromLocal();
    const count=bp.length;
    const dateStr=bd?new Date(bd).toLocaleString('fr-FR'):'Jamais';
    const hasPending=localStorage.getItem(LS_PENDING_KEY);
    const sheetsStr=lastSuccessfulSheetsSync?new Date(lastSuccessfulSheetsSync).toLocaleString('fr-FR'):'Cette session';
    const msg=`💾 ${count} prospects — Local: ${dateStr}${hasPending?' ⚠️ Sync en attente':' ✅'} — Sheets: ${sheetsStr}`;
    showNotif(msg,'info');
  }catch(e){showNotif('Informations de sauvegarde indisponibles','warning');}
}

// ── NOTIF ──
function showNotif(msg,type='success'){
  const el=document.getElementById('notif');
  el.className='notif'+(type==='error'?' error':type==='warning'?' warning':type==='info'?' info':'');
  document.getElementById('notif-ico').textContent=type==='error'?'✗':type==='warning'?'⚠':type==='info'?'ℹ':'✓';
  document.getElementById('notif-text').textContent=msg;
  el.style.display='flex';
  clearTimeout(el._t);
  el._t=setTimeout(()=>el.style.display='none',3500);
}

// ── DRAG DROP IMPORT ──
const iz=document.getElementById('import-zone');
iz.addEventListener('dragover',e=>{e.preventDefault();iz.classList.add('drag');});
iz.addEventListener('dragleave',()=>iz.classList.remove('drag'));
iz.addEventListener('drop',e=>{e.preventDefault();iz.classList.remove('drag');const f=e.dataTransfer.files[0];if(f)handleFileSelect({target:{files:[f]},});});

// ── KEYBOARD ──
document.addEventListener('keydown',e=>{
  if(e.key==='Escape'){
    if(document.querySelector('.overlay.open'))document.querySelectorAll('.overlay.open').forEach(m=>m.classList.remove('open'));
    else if(detailOpen)closeDV();
  }
  if((e.ctrlKey||e.metaKey)&&e.key==='k'){e.preventDefault();document.getElementById('search-input')?.focus();}
});

// ── CLOSE DROPDOWN ON OUTSIDE CLICK ──
document.addEventListener('click',e=>{
  const dd=document.getElementById('prospect-dropdown');
  if(dd&&!dd.contains(e.target)&&e.target.id!=='action-prospect-search')dd.style.display='none';
});

// ── LANDING PAGE ──
function initLandingPage(){
  const params=new URLSearchParams(window.location.search);
  const mode=params.get('mode');
  const readonly=params.get('readonly');
  
  // Si déjà en mode explicite → skip landing
  if(mode==='admin'||mode==='view'||readonly==='1'){
    hideLanding();
    return;
  }
  
  // Afficher la date
  const d=new Date();
  const el=document.getElementById('lp-date');
  if(el)el.textContent=d.toLocaleDateString('fr-FR',{weekday:'long',day:'numeric',month:'long',year:'numeric'});
  
  // Bouton Admin
  document.getElementById('lp-admin-btn').addEventListener('click',function(e){
    e.preventDefault();
    hideLanding('admin');
  });
  
  // Bouton Collaborateur  
  document.getElementById('lp-collab-btn').addEventListener('click',function(e){
    e.preventDefault();
    hideLanding('readonly');
  });
}

function hideLanding(mode){
  const landing=document.getElementById('landing');
  if(!landing)return;
  
  landing.style.transition='opacity .5s ease, transform .5s ease';
  landing.style.opacity='0';
  landing.style.transform='scale(1.04)';
  
  setTimeout(()=>{
    landing.classList.add('hidden');
    if(mode==='readonly'){
      activateReadonlyMode();
    }
    // Mettre à jour les stats landing après chargement
    updateLandingStats();
  },480);
}

function activateReadonlyMode(){
  document.body.classList.add('readonly-mode');
  window.openProspectModal=()=>null;
  window.editProspect=()=>null;
  window.deleteProspect=()=>null;
  window.openActionModal=()=>null;
  window.openImportModal=()=>null;
  window.completeAction=()=>null;
  window.changeCat=()=>null;
  window.handleProspectSubmit=()=>null;
  window.handleActionSubmit=()=>null;
  window.confirmCompleteAction=()=>null;
}

function updateLandingStats(){
  // Met à jour les chiffres dans la landing (si elle était encore visible)
  const chaudEl=document.getElementById('lp-chaud-count');
  const actEl=document.getElementById('lp-actions-count');
  if(chaudEl)chaudEl.textContent=prospects.filter(p=>p.categorie==='chaud').length;
  if(actEl)actEl.textContent=prospects.filter(p=>p.prochaineAction).length;
}

// ── INIT ──
document.addEventListener('DOMContentLoaded',()=>{
  // Détection URL directe (liens partagés)
  const params=new URLSearchParams(window.location.search);
  const isReadonly=params.get('readonly')==='1'||params.get('mode')==='view';
  const isAdmin=params.get('mode')==='admin';
  
  // Init landing page (gère les boutons et l'animation)
  initLandingPage();
  
  // Si readonly via URL directe (lien collaborateur)
  if(isReadonly){
    activateReadonlyMode();
    console.log('👁️ Mode lecture seule activé via URL');
  }
  
  // Charger l'historique des suppressions depuis localStorage
  try{
    const saved=localStorage.getItem('siphro_deleted_prospects');
    if(saved)deletedProspects=JSON.parse(saved);
    console.log(`📋 ${deletedProspects.length} prospects supprimés en historique`);
  }catch(e){console.warn('Erreur chargement suppressions:',e);}
  
  // Charger les données (en arrière-plan pendant que la landing est visible)
  loadData().then(()=>{
    updateLandingStats();
  }).catch(()=>{
    updateLandingStats();
  });
});
</script>
</body>
</html>
