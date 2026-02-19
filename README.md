[index (1).html](https://github.com/user-attachments/files/25416901/index.1.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
.app{display:flex;min-height:100vh}
.sidebar{width:var(--sidebar);background:linear-gradient(180deg,#1a2340 0%,#0d1526 100%);color:#fff;padding:0;position:fixed;height:100vh;overflow-y:auto;z-index:200;display:flex;flex-direction:column}
.main{flex:1;margin-left:var(--sidebar);min-height:100vh;display:flex;flex-direction:column;overflow:hidden}
.view{display:none;flex:1;flex-direction:column;min-height:0}
.view.active{display:flex}

/* ── SIDEBAR ── */
.sb-brand{padding:22px 20px 18px;border-bottom:1px solid rgba(255,255,255,.07)}
.sb-brand-row{display:flex;align-items:center;gap:12px;margin-bottom:12px}
.sb-icon{width:40px;height:40px;background:linear-gradient(135deg,#3b82f6,#8b5cf6);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:18px;box-shadow:0 4px 14px rgba(59,130,246,.4)}
.sb-title{font-family:'Fraunces',serif;font-size:18px;font-weight:700;line-height:1}
.sb-sub{font-size:10px;color:rgba(255,255,255,.4);letter-spacing:1.2px;text-transform:uppercase}
.sb-sync{font-size:11px;padding:6px 10px;background:rgba(255,255,255,.07);border-radius:8px;color:rgba(255,255,255,.55);display:flex;align-items:center;gap:6px}
.sb-nav{padding:16px 12px;flex:1}
.sb-sec{margin-bottom:24px}
.sb-sec-lbl{font-size:9.5px;text-transform:uppercase;letter-spacing:1.8px;color:rgba(255,255,255,.28);padding:0 10px;margin-bottom:6px;font-weight:700}
.ni{display:flex;align-items:center;gap:10px;padding:10px 12px;border-radius:10px;cursor:pointer;transition:all .18s;margin-bottom:1px;color:rgba(255,255,255,.6);font-size:13px;font-weight:500;position:relative}
.ni:hover{background:rgba(255,255,255,.07);color:#fff}
.ni.active{background:rgba(59,130,246,.22);color:#fff;border-left:3px solid #60a5fa}
.ni-ico{font-size:15px;width:20px;text-align:center;flex-shrink:0}
.ni-lbl{flex:1}
.nbadge{margin-left:auto;font-size:10px;font-weight:700;padding:2px 7px;border-radius:10px;min-width:20px;text-align:center;background:rgba(239,68,68,.85);color:#fff}
.nbadge.b-blue{background:rgba(59,130,246,.75)}
.nbadge.b-green{background:rgba(5,150,105,.75)}
.nbadge.b-orange{background:rgba(217,119,6,.8)}
.nbadge.b-gray{background:rgba(100,116,139,.6)}
.dot{width:8px;height:8px;border-radius:50%;flex-shrink:0}
.sb-footer{padding:14px 20px;border-top:1px solid rgba(255,255,255,.07)}
.sb-version{font-size:10px;color:rgba(255,255,255,.22);text-align:center}

/* ── HEADER ── */
.page-hdr{background:var(--bg2);border-bottom:1px solid var(--bl);padding:18px 32px;position:sticky;top:0;z-index:100;flex-shrink:0}
.hdr-row{display:flex;justify-content:space-between;align-items:center;gap:16px}
.page-title{font-family:'Fraunces',serif;font-size:24px;font-weight:700;letter-spacing:-.3px}
.page-sub{color:var(--txt2);font-size:12.5px;margin-top:2px}
.hdr-actions{display:flex;gap:8px;align-items:center;flex-shrink:0}
.sync-pill{font-size:11.5px;font-weight:600;padding:6px 14px;background:var(--bg3);border-radius:20px;border:1px solid var(--bl);white-space:nowrap}

/* ── BUTTONS ── */
.btn{display:inline-flex;align-items:center;gap:6px;padding:10px 18px;border-radius:var(--r2);font-size:13px;font-weight:600;cursor:pointer;transition:all .18s;border:none;font-family:inherit;white-space:nowrap}
.btn-primary{background:var(--blue);color:#fff;box-shadow:0 2px 8px rgba(37,99,235,.3)}.btn-primary:hover{background:var(--blue-d);transform:translateY(-1px);box-shadow:0 4px 14px rgba(37,99,235,.4)}
.btn-secondary{background:var(--bg3);color:var(--txt);border:1.5px solid var(--bl)}.btn-secondary:hover{background:var(--bl)}
.btn-green{background:var(--green);color:#fff;box-shadow:0 2px 8px rgba(5,150,105,.3)}.btn-green:hover{background:#047857}
.btn-danger{background:var(--red-l);color:var(--red);border:1px solid #fecaca}.btn-danger:hover{background:var(--red);color:#fff}
.btn-sm{padding:7px 13px;font-size:12px}
.btn-xs{padding:5px 10px;font-size:11px}
.ibt{width:30px;height:30px;border:none;background:transparent;border-radius:8px;cursor:pointer;font-size:13px;transition:all .18s;display:flex;align-items:center;justify-content:center;color:var(--txt2)}
.ibt:hover{background:var(--bl)}.ibt.del:hover{background:var(--red-l);color:var(--red)}

/* ── STATS BAR ── */
.stats-bar{display:grid;grid-template-columns:repeat(5,1fr);gap:12px;padding:18px 32px;background:var(--bg2);border-bottom:1px solid var(--bl);flex-shrink:0}
.stat{background:var(--bg);border:1.5px solid var(--bl);border-radius:var(--r3);padding:16px 18px;position:relative;overflow:hidden;cursor:pointer;transition:all .2s}
.stat:hover{box-shadow:var(--sh2);transform:translateY(-2px);border-color:var(--bm)}
.stat.active-filter{border-color:var(--blue);background:var(--blue-l)}
.stat::before{content:'';position:absolute;top:0;left:0;width:3px;height:100%;border-radius:0 2px 2px 0}
.stat.s-total::before{background:var(--blue)}.stat.s-retard::before{background:var(--red)}.stat.s-today::before{background:var(--orange)}.stat.s-nego::before{background:var(--green)}.stat.s-orga::before{background:var(--purple)}
.stat-val{font-size:28px;font-weight:800;line-height:1;margin-bottom:3px}
.stat-lbl{font-size:10.5px;color:var(--txt2);text-transform:uppercase;letter-spacing:.5px;font-weight:600}
.stat-ico{position:absolute;right:12px;top:50%;transform:translateY(-50%);font-size:30px;opacity:.1}

/* ── FILTERS BAR ── */
.filters-bar{display:flex;gap:10px;padding:14px 32px;background:var(--bg2);border-bottom:1px solid var(--bl);align-items:center;flex-wrap:wrap;flex-shrink:0}
.search-wrap{flex:1;min-width:240px;position:relative}
.search-wrap input{width:100%;padding:11px 16px 11px 42px;border:1.5px solid var(--bl);border-radius:var(--r4);font-size:13.5px;font-family:inherit;background:var(--bg);transition:all .2s;color:var(--txt)}
.search-wrap input:focus{outline:none;border-color:var(--blue);background:#fff;box-shadow:0 0 0 3px rgba(37,99,235,.08)}
.search-wrap .si{position:absolute;left:14px;top:50%;transform:translateY(-50%);color:var(--txt3);font-size:16px}
.search-wrap .clear-btn{position:absolute;right:12px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;color:var(--txt3);font-size:18px;display:none;padding:0;line-height:1}
.search-wrap input:not(:placeholder-shown)~.clear-btn{display:block}
.ftabs{display:flex;gap:4px;background:var(--bg3);padding:4px;border-radius:var(--r3);flex-wrap:wrap}
.ftab{padding:8px 14px;border-radius:var(--r2);font-size:12px;font-weight:600;cursor:pointer;transition:all .18s;background:transparent;border:none;color:var(--txt2);display:flex;align-items:center;gap:6px;font-family:inherit;white-space:nowrap}
.ftab:hover{color:var(--txt);background:rgba(255,255,255,.5)}
.ftab.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.ftab .cnt{background:var(--bg4);padding:1px 7px;border-radius:8px;font-size:10px;font-weight:700}
.ftab.active .cnt{background:var(--blue-l);color:var(--blue)}
.sort-sel{padding:9px 12px;border:1.5px solid var(--bl);border-radius:var(--r2);font-size:12.5px;font-family:inherit;background:var(--bg);cursor:pointer;color:var(--txt);outline:none}
.sort-sel:focus{border-color:var(--blue)}
.view-tog{display:flex;gap:3px;background:var(--bg3);padding:3px;border-radius:var(--r2)}
.vtb{width:32px;height:32px;border:none;background:transparent;border-radius:8px;cursor:pointer;font-size:15px;display:flex;align-items:center;justify-content:center;color:var(--txt2);transition:all .18s}
.vtb.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}

/* ── PAGE CONTENT ── */
.page-content{padding:24px 32px;flex:1;overflow-y:auto}
.prospects-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(360px,1fr));gap:16px}
.prospects-list{display:flex;flex-direction:column;gap:8px}

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

/* ── AGENDA ── */
.agenda-nav{display:flex;align-items:center;gap:14px;padding:14px 32px;background:var(--bg2);border-bottom:1px solid var(--bl);flex-shrink:0;flex-wrap:wrap}
.week-nav{display:flex;align-items:center;gap:8px}
.week-nav button{padding:7px 12px;border:1.5px solid var(--bl);background:var(--bg2);border-radius:var(--r2);cursor:pointer;font-weight:600;font-size:12px;font-family:inherit;transition:all .2s;color:var(--txt)}
.week-nav button:hover{background:var(--bg3);border-color:var(--bm)}
.week-lbl{font-size:14px;font-weight:700;min-width:190px;text-align:center;color:var(--txt)}
.ag-toggle{display:flex;gap:4px;background:var(--bg3);padding:3px;border-radius:var(--r2);margin-left:auto}
.ag-btn{padding:7px 14px;border:none;background:transparent;border-radius:8px;cursor:pointer;font-size:12.5px;font-weight:600;font-family:inherit;color:var(--txt2);transition:all .2s}
.ag-btn.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.agenda-body{flex:1;overflow:auto;padding:0 32px 32px}

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
@media(max-width:1100px){
  .dv-body{grid-template-columns:1fr}
  .dv-aside{flex-direction:row;flex-wrap:wrap}
  .dv-aside>*{flex:1;min-width:280px}
}
@media(max-width:900px){
  :root{--sidebar:220px}
  .stats-bar{grid-template-columns:repeat(3,1fr)}
  .dv-info-grid{grid-template-columns:repeat(2,1fr)}
}
@media(max-width:680px){
  :root{--sidebar:0px}
  .sidebar{display:none}
  .main{margin-left:0}
  .page-content,.filters-bar,.stats-bar,.page-hdr,.agenda-body{padding-left:16px;padding-right:16px}
  .stats-bar{grid-template-columns:1fr 1fr}
  .frow{grid-template-columns:1fr}
}

/* ── SCORE RING ── */
.score-ring{display:flex;align-items:center;gap:8px;font-size:12px;font-weight:600;color:var(--txt2)}
.score-dots{display:flex;gap:3px}
.sdot{width:8px;height:8px;border-radius:50%;background:var(--bl)}
.sdot.on.red{background:var(--red)}.sdot.on.orange{background:var(--orange)}.sdot.on.green{background:var(--green)}
</style>
</head>
<body>
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
      <div class="ni" onclick="openImportModal()"><span class="ni-ico">📥</span><span class="ni-lbl">Importer CSV/Excel</span></div>
      <div class="ni" onclick="exportData()"><span class="ni-ico">📤</span><span class="ni-lbl">Exporter CSV</span></div>
    </div>
  </nav>
  <div class="sb-footer"><div class="sb-version">SIPHRO v2.0 — CHR Méditerranée</div></div>
</aside>

<!-- ══ MAIN ══ -->
<main class="main">

<!-- ── DASHBOARD VIEW ── -->
<div class="view active" id="view-dashboard">
  <!-- Fiche client (detail view) à l'intérieur du dashboard -->
  <div id="detail-view" class="detail-view"></div>
  <!-- Contenu principal dashboard -->
  <div id="dash-content">
    <header class="page-hdr">
      <div class="hdr-row">
        <div>
          <h1 class="page-title">Tableau de bord</h1>
          <p class="page-sub">Gérez vos prospects et relances commerciales</p>
        </div>
        <div class="hdr-actions">
          <span class="sync-pill" id="sync-badge">🔄 Chargement...</span>
          <button class="btn btn-secondary btn-sm" onclick="openImportModal()">📥 Importer</button>
          <button class="btn btn-primary btn-sm" onclick="openProspectModal()">+ Nouveau prospect</button>
        </div>
      </div>
    </header>
    <div class="stats-bar">
      <div class="stat s-total" id="stat-total-box" onclick="applyFilter('all')"><div class="stat-val" id="stat-total">0</div><div class="stat-lbl">Total Prospects</div><span class="stat-ico">📋</span></div>
      <div class="stat s-retard" id="stat-overdue-box" onclick="applyFilter('overdue')"><div class="stat-val" id="stat-retard">0</div><div class="stat-lbl">En retard</div><span class="stat-ico">⚠️</span></div>
      <div class="stat s-today" id="stat-today-box" onclick="applyFilter('today')"><div class="stat-val" id="stat-today">0</div><div class="stat-lbl">Aujourd'hui</div><span class="stat-ico">📅</span></div>
      <div class="stat s-nego" id="stat-nego-box" onclick="applyFilter('negociation')"><div class="stat-val" id="stat-nego">0</div><div class="stat-lbl">En négociation</div><span class="stat-ico">🤝</span></div>
      <div class="stat s-orga" id="stat-orga-box" onclick="applyFilter('organisation')"><div class="stat-val" id="stat-orga">0</div><div class="stat-lbl">En organisation</div><span class="stat-ico">📋</span></div>
    </div>
    <div class="filters-bar">
      <div class="search-wrap">
        <span class="si">🔍</span>
        <input type="text" id="search-input" placeholder="Rechercher nom, contact, ville, email, téléphone, formation..." oninput="handleSearch(this.value)" autocomplete="off">
        <button class="clear-btn" onclick="clearSearch()">×</button>
      </div>
      <div class="ftabs" id="filter-tabs">
        <button class="ftab active" onclick="setFilter('all')" data-f="all">Tous <span class="cnt" id="fc-all">0</span></button>
        <button class="ftab" onclick="setFilter('organisation')" data-f="organisation">📋 Orga <span class="cnt" id="fc-organisation">0</span></button>
        <button class="ftab" onclick="setFilter('negociation')" data-f="negociation">🤝 Négo <span class="cnt" id="fc-negociation">0</span></button>
        <button class="ftab" onclick="setFilter('chaud')" data-f="chaud">🔥 Chaud <span class="cnt" id="fc-chaud">0</span></button>
        <button class="ftab" onclick="setFilter('autre')" data-f="autre">📌 Autre <span class="cnt" id="fc-autre">0</span></button>
        <button class="ftab" onclick="setFilter('gagne')" data-f="gagne">✅ Gagné <span class="cnt" id="fc-gagne">0</span></button>
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
    <div class="page-content" id="page-content">
      <div id="prospects-container"></div>
      <div class="empty" id="empty-state" style="display:none">
        <div class="empty-ico">📂</div>
        <h2 class="empty-title">Aucun prospect trouvé</h2>
        <p class="empty-sub">Modifiez votre recherche ou ajoutez un nouveau prospect</p>
        <div style="display:flex;gap:10px;justify-content:center">
          <button class="btn btn-secondary btn-sm" onclick="clearSearch()">🔄 Effacer la recherche</button>
          <button class="btn btn-primary btn-sm" onclick="openProspectModal()">+ Ajouter</button>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ── AGENDA VIEW ── -->
<div class="view" id="view-agenda">
  <header class="page-hdr">
    <div class="hdr-row">
      <div><h1 class="page-title">Agenda</h1><p class="page-sub">Vos relances et actions planifiées par créneaux horaires</p></div>
      <div class="hdr-actions">
        <span class="sync-pill" id="sync-badge2">✅ Sync</span>
        <button class="btn btn-primary btn-sm" onclick="openActionModal(null,true)">+ Nouvelle tâche</button>
      </div>
    </div>
  </header>
  <div class="agenda-nav">
    <div class="week-nav">
      <button onclick="agNav(-1)">← Préc</button>
      <div class="week-lbl" id="ag-label">...</div>
      <button onclick="agNav(1)">Suiv →</button>
      <button onclick="agToday()" style="background:var(--blue-l);color:var(--blue);border-color:var(--blue)">Aujourd'hui</button>
    </div>
    <div class="ag-toggle">
      <button class="ag-btn" id="ag-list" onclick="setAgView('list')">📋 Liste</button>
      <button class="ag-btn active" id="ag-week" onclick="setAgView('week')">📅 Semaine</button>
      <button class="ag-btn" id="ag-day" onclick="setAgView('day')">🕐 Jour</button>
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

<!-- Notif toast -->
<div class="notif" id="notif"><span id="notif-ico">✓</span><span id="notif-text"></span></div>

<script>
// ══════════════════════════════════════════════
//  SIPHRO CRM v2.0 — Core JavaScript
// ══════════════════════════════════════════════

const SCRIPT_URL='https://script.google.com/macros/s/AKfycbyn_49XC_MOUfCt714IP_oSxhGNGUn9n1M6xm5AsdHiWKN-XRdeS8cCQAcSuG8v0dXMnA/exec';
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
let syncTimer=null;
let detailOpen=false;
let completePid=null;

// ── SYNC STATUS ──
function setSyncStatus(msg,color){
  ['sb-sync','sync-badge','sync-badge2'].forEach(id=>{
    const e=document.getElementById(id);
    if(e){e.textContent=msg;if(color)e.style.color=color;}
  });
}

// ── GOOGLE SHEETS ──
async function loadData(){
  setSyncStatus('🔄 Chargement...','#d97706');
  try{
    const res=await fetch(SCRIPT_URL);
    const raw=await res.json();
    prospects=raw.map(parseRow).filter(p=>p.nom).map(enrichProspect);
    setSyncStatus('✅ Synchronisé','#059669');
  }catch(e){
    setSyncStatus('❌ Connexion impossible','#dc2626');
    showNotif('Impossible de charger depuis Google Sheets','error');
    prospects=[];
  }
  renderAll();
  if(!syncTimer)syncTimer=setInterval(autoRefresh,20000);
}

async function autoRefresh(){
  try{
    const res=await fetch(SCRIPT_URL+'?t='+Date.now());
    const raw=await res.json();
    const remote=raw.map(parseRow).filter(p=>p.nom).map(enrichProspect);
    if(JSON.stringify(remote)!==JSON.stringify(prospects)){
      prospects=remote;
      renderAll();
      if(detailOpen){
        const dv=document.getElementById('detail-view');
        // Refresh detail if open
        const pid=dv.dataset.pid;
        if(pid&&prospects.find(p=>p.id===pid))openDetailView(pid);
      }
      setSyncStatus('🔄 Mis à jour','#2563eb');
      setTimeout(()=>setSyncStatus('✅ Synchronisé','#059669'),2000);
    }
  }catch(e){}
}

async function saveData(){
  if(isSaving)return;
  isSaving=true;
  setSyncStatus('💾 Sauvegarde...','#d97706');
  try{
    await fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'application/json'},body:JSON.stringify({action:'save_all',data:prospects})});
    setSyncStatus('✅ Sauvegardé','#059669');
  }catch(e){
    setSyncStatus('❌ Erreur sauvegarde','#dc2626');
    showNotif('Erreur de sauvegarde','error');
  }
  isSaving=false;
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
  // Works from sidebar AND stat cards — always goes to dashboard
  showView('dashboard');
  if(['overdue','today','week'].includes(f)){
    filterMode='urgency';currentFilter=f;
    document.querySelectorAll('.ftab').forEach(t=>t.classList.remove('active'));
  }else{
    filterMode='cat';currentFilter=f;
    document.querySelectorAll('.ftab').forEach(t=>t.classList.toggle('active',t.dataset.f===f));
  }
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
  <div class="card-acts" onclick="event.stopPropagation()">
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
<div class="card-acts" onclick="event.stopPropagation()">
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
  if(!confirm(`Supprimer "${p.nom}" ?\nCette action est irréversible.`))return;
  prospects=prospects.filter(p=>p.id!==id);
  saveData();closeDV();renderAll();
  showNotif('Prospect supprimé','warning');
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
  const csv=Papa.unparse(prospects.map(p=>({
    nom_etablissement:p.nom,contact_nom:p.contactNom||'',contact_telephone:p.contactTel||'',contact_email:p.contactEmail||'',adresse:p.adresse||'',siret:p.siret||'',categorie:p.categorie,formation_visee:p.formation||'',commentaire:p.commentaire||'',prochaine_action:p.prochaineAction?.description||'',date_prochaine_action:p.prochaineAction?.date||'',actions_effectuees_count:p.actionsEffectuees?.length||0,derniere_action:p.actionsEffectuees?.length?p.actionsEffectuees[p.actionsEffectuees.length-1].description:''
  })),{delimiter:';'});
  const blob=new Blob(['\ufeff'+csv],{type:'text/csv;charset=utf-8'});
  const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download='siphro-export-'+new Date().toISOString().split('T')[0]+'.csv';a.click();URL.revokeObjectURL(url);
  showNotif('Export CSV téléchargé ✓');
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

// ── INIT ──
document.addEventListener('DOMContentLoaded',loadData);
</script>
</body>
</html>
