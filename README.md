[index (1).html](https://github.com/user-attachments/files/25358802/index.1.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SIPHRO — Suivi Commercial</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Fraunces:opsz,wght@9..144,600;9..144,700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
:root{--bg:#f5f6fa;--bg2:#fff;--bg3:#f0f2f8;--txt:#1a1d26;--txt2:#5c6370;--txt3:#9ca3af;--bl:#e8ebef;--bm:#d1d5db;--blue:#2563eb;--blue-l:#dbeafe;--blue-d:#1d4ed8;--green:#059669;--green-l:#d1fae5;--orange:#d97706;--orange-l:#fef3c7;--red:#dc2626;--red-l:#fee2e2;--purple:#7c3aed;--purple-l:#ede9fe;--sh1:0 1px 3px rgba(0,0,0,.05);--sh2:0 4px 16px rgba(0,0,0,.08);--sh3:0 12px 40px rgba(0,0,0,.12);--r1:6px;--r2:10px;--r3:16px;--r4:24px}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Plus Jakarta Sans',sans-serif;background:var(--bg);color:var(--txt);line-height:1.6;min-height:100vh}
.app{display:flex;min-height:100vh}
.sidebar{width:270px;background:linear-gradient(180deg,#1e293b,#0f172a);color:#fff;padding:20px 0;position:fixed;height:100vh;overflow-y:auto;z-index:100}
.main{flex:1;margin-left:270px;min-height:100vh;display:flex;flex-direction:column}
.sb-logo{padding:0 20px 24px;border-bottom:1px solid rgba(255,255,255,.08);margin-bottom:20px}
.sb-logo-row{display:flex;align-items:center;gap:12px}
.sb-icon{width:42px;height:42px;background:linear-gradient(135deg,#3b82f6,#8b5cf6);border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:20px;box-shadow:0 4px 16px rgba(59,130,246,.4)}
.sb-title{font-family:'Fraunces',serif;font-size:17px;font-weight:700}
.sb-sub{font-size:10px;color:rgba(255,255,255,.45);text-transform:uppercase;letter-spacing:1px}
.sb-sync{margin-top:10px;font-size:11px;padding:6px 10px;background:rgba(255,255,255,.08);border-radius:8px;color:rgba(255,255,255,.6)}
.sb-nav{padding:0 12px}
.sb-section{margin-bottom:28px}
.sb-sec-title{font-size:10px;text-transform:uppercase;letter-spacing:1.5px;color:rgba(255,255,255,.3);padding:0 8px;margin-bottom:8px;font-weight:700}
.nav-item{display:flex;align-items:center;gap:10px;padding:11px 12px;border-radius:var(--r2);cursor:pointer;transition:all .2s;margin-bottom:2px;color:rgba(255,255,255,.65);font-size:13.5px;font-weight:500}
.nav-item:hover{background:rgba(255,255,255,.08);color:#fff}
.nav-item.active{background:rgba(59,130,246,.25);color:#fff;border-left:3px solid #3b82f6}
.ni{font-size:16px;width:22px;text-align:center}
.badge{margin-left:auto;background:rgba(239,68,68,.9);color:#fff;font-size:10px;font-weight:700;padding:2px 7px;border-radius:10px;min-width:20px;text-align:center}
.badge.blue{background:rgba(37,99,235,.8)}.badge.green{background:rgba(5,150,105,.8)}.badge.orange{background:rgba(217,119,6,.8)}
.dot8{width:8px;height:8px;border-radius:50%}
.view{display:none;flex:1;flex-direction:column}
.view.active{display:flex}
.page-hdr{background:var(--bg2);border-bottom:1px solid var(--bl);padding:20px 36px;position:sticky;top:0;z-index:50}
.hdr-row{display:flex;justify-content:space-between;align-items:center}
.page-title{font-family:'Fraunces',serif;font-size:26px;font-weight:700;letter-spacing:-.4px}
.page-sub{color:var(--txt2);font-size:13px;margin-top:2px}
.hdr-actions{display:flex;gap:10px;align-items:center}
.btn{display:inline-flex;align-items:center;gap:7px;padding:11px 18px;border-radius:var(--r2);font-size:13.5px;font-weight:600;cursor:pointer;transition:all .2s;border:none;font-family:inherit}
.btn-primary{background:var(--blue);color:#fff;box-shadow:0 4px 12px rgba(37,99,235,.25)}.btn-primary:hover{background:var(--blue-d);transform:translateY(-1px)}
.btn-secondary{background:var(--bg3);color:var(--txt);border:1px solid var(--bl)}.btn-secondary:hover{background:var(--bl)}
.btn-danger{background:var(--red-l);color:var(--red);border:1px solid #fecaca}.btn-danger:hover{background:var(--red);color:#fff}
.btn-sm{padding:8px 14px;font-size:12.5px}
.sync-badge{font-size:12px;font-weight:600;padding:7px 14px;background:var(--bg3);border-radius:20px;border:1px solid var(--bl);white-space:nowrap}
.stats-bar{display:grid;grid-template-columns:repeat(5,1fr);gap:14px;padding:20px 36px;background:var(--bg2);border-bottom:1px solid var(--bl)}
.stat{background:var(--bg3);border-radius:var(--r3);padding:18px 20px;position:relative;overflow:hidden;cursor:pointer;transition:all .2s}
.stat:hover{box-shadow:var(--sh2);transform:translateY(-2px)}
.stat::before{content:'';position:absolute;top:0;left:0;width:4px;height:100%}
.stat.s-total::before{background:var(--blue)}.stat.s-retard::before{background:var(--red)}.stat.s-today::before{background:var(--orange)}.stat.s-nego::before{background:var(--green)}.stat.s-orga::before{background:var(--purple)}
.stat-val{font-size:30px;font-weight:800;line-height:1;margin-bottom:4px}
.stat-lbl{font-size:11px;color:var(--txt2);text-transform:uppercase;letter-spacing:.5px;font-weight:600}
.stat-ico{position:absolute;right:14px;top:50%;transform:translateY(-50%);font-size:34px;opacity:.12}
.filters-bar{display:flex;gap:12px;padding:16px 36px;background:var(--bg2);border-bottom:1px solid var(--bl);align-items:center;flex-wrap:wrap}
.search-wrap{flex:1;min-width:260px;position:relative}
.search-wrap input{width:100%;padding:13px 18px 13px 44px;border:2px solid var(--bl);border-radius:var(--r3);font-size:13.5px;font-family:inherit;background:var(--bg);transition:all .2s}
.search-wrap input:focus{outline:none;border-color:var(--blue);background:#fff;box-shadow:0 0 0 4px rgba(37,99,235,.1)}
.search-wrap .si{position:absolute;left:15px;top:50%;transform:translateY(-50%);color:var(--txt3);font-size:17px}
.ftabs{display:flex;gap:5px;background:var(--bg3);padding:5px;border-radius:var(--r3)}
.ftab{padding:9px 15px;border-radius:var(--r2);font-size:12.5px;font-weight:600;cursor:pointer;transition:all .2s;background:transparent;border:none;color:var(--txt2);display:flex;align-items:center;gap:7px;font-family:inherit}
.ftab:hover{color:var(--txt)}.ftab.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.ftab .cnt{background:var(--bg3);padding:1px 7px;border-radius:8px;font-size:10.5px}.ftab.active .cnt{background:var(--blue-l);color:var(--blue)}
.sort-wrap select{padding:10px 14px;border:2px solid var(--bl);border-radius:var(--r2);font-size:13px;font-family:inherit;background:var(--bg);cursor:pointer;color:var(--txt)}
.sort-wrap select:focus{outline:none;border-color:var(--blue)}
.view-toggle{display:flex;gap:4px;background:var(--bg3);padding:4px;border-radius:var(--r2)}
.vt-btn{width:34px;height:34px;border:none;background:transparent;border-radius:var(--r1);cursor:pointer;font-size:16px;display:flex;align-items:center;justify-content:center;color:var(--txt2);transition:all .2s}
.vt-btn.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.page-content{padding:28px 36px;flex:1}
.prospects-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(400px,1fr));gap:20px}
.prospects-list{display:flex;flex-direction:column;gap:10px}
.card{background:var(--bg2);border-radius:var(--r4);border:1px solid var(--bl);overflow:hidden;transition:all .3s;cursor:pointer}
.card:hover{box-shadow:var(--sh3);transform:translateY(-3px);border-color:var(--bm)}
.card-hdr{padding:20px 20px 14px;display:flex;justify-content:space-between;align-items:flex-start;border-bottom:1px solid var(--bl)}
.card-name{font-size:17px;font-weight:700;margin-bottom:5px}
.cbadge{padding:3px 10px;border-radius:16px;font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.5px}
.cbadge.organisation{background:var(--green-l);color:var(--green)}.cbadge.negociation{background:var(--blue-l);color:var(--blue)}.cbadge.chaud{background:var(--orange-l);color:var(--orange)}.cbadge.autre{background:var(--purple-l);color:var(--purple)}.cbadge.gagne{background:#dcfce7;color:#16a34a}
.card-formation{font-size:12px;color:var(--txt2);background:var(--bg3);padding:4px 10px;border-radius:5px;margin-top:6px;display:inline-flex;align-items:center;gap:5px}
.card-acts{display:flex;gap:5px}
.ibt{width:32px;height:32px;border:none;background:var(--bg3);border-radius:var(--r2);cursor:pointer;font-size:14px;transition:all .2s;display:flex;align-items:center;justify-content:center}
.ibt:hover{background:var(--bl)}.ibt.del:hover{background:var(--red-l)}
.card-body{padding:16px 20px}
.ci-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:12px}
.ci{display:flex;align-items:center;gap:8px;font-size:12.5px;color:var(--txt2)}
.ci .ico{width:28px;height:28px;background:var(--bg3);border-radius:5px;display:flex;align-items:center;justify-content:center;font-size:13px;flex-shrink:0}
.ci span{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.cnotes{font-size:12.5px;color:var(--txt2);background:var(--bg3);padding:10px 14px;border-radius:var(--r2);margin-bottom:12px;border-left:3px solid var(--bm);display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden}
.abox{padding:14px;border-radius:var(--r3);margin-bottom:10px}
.abox.overdue{background:linear-gradient(135deg,#fef2f2,#fee2e2);border:1px solid #fecaca}
.abox.today{background:linear-gradient(135deg,#fffbeb,#fef3c7);border:1px solid #fde68a}
.abox.soon{background:linear-gradient(135deg,#eff6ff,#dbeafe);border:1px solid #bfdbfe}
.abox.normal{background:var(--bg3);border:1px solid var(--bl)}
.abox-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
.aurge{font-size:9.5px;font-weight:800;text-transform:uppercase;letter-spacing:1px}
.abox.overdue .aurge{color:var(--red)}.abox.today .aurge{color:var(--orange)}.abox.soon .aurge{color:var(--blue)}.abox.normal .aurge{color:var(--txt2)}
.adate{font-size:11.5px;font-weight:600;color:var(--txt2)}
.atime{font-size:11px;color:var(--blue);font-weight:600}
.adesc{font-size:13.5px;font-weight:500;color:var(--txt);margin-bottom:10px;line-height:1.4}
.acomplete{width:100%;padding:9px;background:#fff;border:2px solid var(--green);border-radius:var(--r2);color:var(--green);font-size:12.5px;font-weight:700;cursor:pointer;transition:all .2s;font-family:inherit}
.acomplete:hover{background:var(--green);color:#fff}
.no-action{border:2px dashed var(--bl);border-radius:var(--r3);padding:16px;text-align:center}
.add-action-btn{background:none;border:none;color:var(--blue);font-size:13.5px;font-weight:600;cursor:pointer;display:flex;align-items:center;gap:7px;margin:0 auto}
.add-action-btn:hover{text-decoration:underline}
.hist-section{border-top:1px solid var(--bl);padding-top:12px}
.hist-toggle{display:flex;align-items:center;gap:7px;font-size:12.5px;font-weight:600;color:var(--txt2);cursor:pointer;background:none;border:none;width:100%;text-align:left;padding:6px 0;font-family:inherit}
.hist-toggle:hover{color:var(--txt)}.hist-toggle .arr{transition:transform .2s}.hist-toggle.open .arr{transform:rotate(90deg)}
.hist-list{display:none;margin-top:8px}.hist-list.open{display:block}
.hist-item{display:flex;gap:10px;padding:10px 0;border-bottom:1px solid var(--bl)}.hist-item:last-child{border-bottom:none}
.hdot{width:8px;height:8px;background:var(--green);border-radius:50%;margin-top:5px;flex-shrink:0}
.hdate{font-size:10.5px;color:var(--txt3);font-weight:600;margin-bottom:3px}.htext{font-size:12.5px;color:var(--txt)}
.list-row{background:var(--bg2);border:1px solid var(--bl);border-radius:var(--r3);padding:16px 20px;display:grid;grid-template-columns:2fr 1fr 1fr 1fr 160px auto;gap:16px;align-items:center;cursor:pointer;transition:all .2s}
.list-row:hover{box-shadow:var(--sh2);border-color:var(--bm)}
.lr-name{font-weight:700;font-size:15px}
.empty{text-align:center;padding:80px 40px;color:var(--txt2)}
.empty-ico{font-size:64px;margin-bottom:20px;opacity:.6}.empty-title{font-family:'Fraunces',serif;font-size:24px;color:var(--txt);margin-bottom:10px}.empty-sub{font-size:14px;margin-bottom:28px}
</style>
<style>
.agenda-container{flex:1;display:flex;flex-direction:column}
.agenda-nav{display:flex;align-items:center;gap:16px;padding:16px 36px;background:var(--bg2);border-bottom:1px solid var(--bl)}
.week-nav{display:flex;align-items:center;gap:10px}
.week-nav button{padding:8px 14px;border:1px solid var(--bl);background:var(--bg2);border-radius:var(--r2);cursor:pointer;font-weight:600;font-size:13px;font-family:inherit;transition:all .2s}
.week-nav button:hover{background:var(--bg3)}
.week-label{font-size:15px;font-weight:700;min-width:200px;text-align:center}
.at-toggle{display:flex;gap:5px;background:var(--bg3);padding:4px;border-radius:var(--r2);margin-left:auto}
.at-btn{padding:8px 16px;border:none;background:transparent;border-radius:var(--r1);cursor:pointer;font-size:13px;font-weight:600;font-family:inherit;color:var(--txt2);transition:all .2s}
.at-btn.active{background:#fff;color:var(--txt);box-shadow:var(--sh1)}
.agenda-grid{flex:1;overflow-x:auto;padding:0 36px 36px}
.week-grid{display:grid;grid-template-columns:70px repeat(7,1fr);min-width:900px}
.wg-corner{border-bottom:1px solid var(--bl);padding:12px 0}
.wg-day-hdr{padding:12px 8px;text-align:center;border-bottom:1px solid var(--bl);border-left:1px solid var(--bl)}
.wg-day-name{font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:.5px;color:var(--txt2)}
.wg-day-num{font-size:22px;font-weight:800;line-height:1;margin-top:4px}
.wg-day-hdr.today-col .wg-day-num{color:var(--blue)}.wg-day-hdr.today-col{background:var(--blue-l)}
.wg-time{padding:0 10px;font-size:11px;color:var(--txt3);font-weight:600;height:60px;display:flex;align-items:flex-start;padding-top:6px;border-bottom:1px solid var(--bl)}
.wg-cell{border-left:1px solid var(--bl);border-bottom:1px solid var(--bl);height:60px;position:relative;padding:2px}
.wg-cell.today-col{background:rgba(37,99,235,.02)}
.wg-event{background:var(--blue);color:#fff;border-radius:5px;padding:3px 7px;font-size:11px;font-weight:600;cursor:pointer;position:absolute;left:2px;right:2px;top:2px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;transition:opacity .2s;z-index:2}
.wg-event:hover{opacity:.85}
.wg-event.overdue{background:var(--red)}.wg-event.today{background:var(--orange)}.wg-event.soon{background:#3b82f6}.wg-event.normal{background:#64748b}
.wg-add{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;opacity:0;cursor:pointer;font-size:20px;color:var(--txt3)}
.wg-cell:hover .wg-add{opacity:1}
.day-grid{display:grid;grid-template-columns:70px 1fr;min-width:600px}
.day-slot{height:60px;border-bottom:1px solid var(--bl);border-left:1px solid var(--bl);position:relative;padding:2px}
.day-time{height:60px;display:flex;align-items:flex-start;padding-top:6px;font-size:11px;color:var(--txt3);font-weight:600;border-bottom:1px solid var(--bl);padding-left:10px}
.day-event{background:var(--blue);color:#fff;border-radius:6px;padding:4px 10px;font-size:12px;font-weight:600;cursor:pointer;margin-bottom:2px;display:flex;align-items:center;gap:8px;transition:opacity .2s}
.day-event:hover{opacity:.85}.day-event.overdue{background:var(--red)}.day-event.today-ev{background:var(--orange)}.day-event.soon{background:#3b82f6}.day-event.normal{background:#64748b}
.agenda-list-wrap{padding:20px 36px 36px}
.adg{margin-bottom:28px}
.adg-title{font-size:16px;font-weight:800;padding-bottom:10px;border-bottom:2px solid var(--bl);margin-bottom:12px;display:flex;align-items:center;gap:10px}
.adg-tag{font-size:10px;padding:2px 8px;border-radius:10px;font-weight:700;color:#fff}
.ai{background:var(--bg2);border:1px solid var(--bl);border-radius:var(--r3);padding:14px 18px;margin-bottom:8px;display:flex;align-items:center;gap:16px;cursor:pointer;transition:all .2s}
.ai:hover{box-shadow:var(--sh2);border-color:var(--bm)}
.ai-time{font-size:13px;font-weight:700;color:var(--blue);min-width:55px}
.ai-dot{width:12px;height:12px;border-radius:50%;flex-shrink:0}
.ai-prospect{font-weight:700;font-size:14px}
.ai-action{font-size:13px;color:var(--txt2);margin-top:2px}
.cpl-btn{padding:7px 14px;background:var(--green-l);border:1px solid var(--green);border-radius:var(--r2);color:var(--green);font-size:12px;font-weight:700;cursor:pointer;font-family:inherit;white-space:nowrap;transition:all .2s}
.cpl-btn:hover{background:var(--green);color:#fff}
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.5);backdrop-filter:blur(4px);display:none;align-items:center;justify-content:center;z-index:1000;padding:20px}
.modal-overlay.open{display:flex}
.modal{background:var(--bg2);border-radius:var(--r4);width:100%;max-width:680px;max-height:92vh;overflow:hidden;box-shadow:var(--sh3);animation:mIn .3s ease}
.modal-sm{max-width:500px}
@keyframes mIn{from{opacity:0;transform:translateY(16px) scale(.98)}to{opacity:1;transform:none}}
.modal-hdr{padding:22px 28px;border-bottom:1px solid var(--bl);display:flex;justify-content:space-between;align-items:center;background:var(--bg3)}
.modal-title{font-family:'Fraunces',serif;font-size:21px;font-weight:700}
.modal-close{width:38px;height:38px;border:none;background:#fff;border-radius:var(--r2);font-size:22px;cursor:pointer;color:var(--txt2);display:flex;align-items:center;justify-content:center;transition:all .2s}
.modal-close:hover{background:var(--bl)}
.modal-body{padding:28px;overflow-y:auto;max-height:calc(92vh - 170px)}
.modal-ftr{padding:18px 28px;border-top:1px solid var(--bl);display:flex;gap:10px;justify-content:flex-end;background:var(--bg3)}
.fsec{margin-bottom:24px}
.fsec-title{font-size:13.5px;font-weight:700;margin-bottom:14px;display:flex;align-items:center;gap:9px}
.fsec-ico{width:26px;height:26px;background:var(--blue-l);color:var(--blue);border-radius:5px;display:flex;align-items:center;justify-content:center;font-size:13px}
.frow{display:grid;grid-template-columns:repeat(2,1fr);gap:14px;margin-bottom:14px}
.fg{display:flex;flex-direction:column;gap:7px}
.fg.full{grid-column:1/-1}
.flbl{font-size:12.5px;font-weight:600;color:var(--txt2)}
.finp,.fsel,.ftxt{padding:12px 14px;border:2px solid var(--bl);border-radius:var(--r2);font-size:13.5px;font-family:inherit;transition:all .2s;background:var(--bg)}
.finp:focus,.fsel:focus,.ftxt:focus{outline:none;border-color:var(--blue);background:#fff}
.ftxt{resize:vertical;min-height:75px}
.cat-sel{display:grid;grid-template-columns:repeat(2,1fr);gap:8px}
.cat-opt{padding:12px 14px;border:2px solid var(--bl);border-radius:var(--r2);cursor:pointer;transition:all .2s;display:flex;align-items:center;gap:8px;font-size:13px;font-weight:600;background:var(--bg)}
.cat-opt:hover{border-color:var(--bm)}.cat-opt.selected{border-color:var(--blue);background:var(--blue-l)}
.cat-dot{width:11px;height:11px;border-radius:50%}
.import-zone{border:3px dashed var(--bl);border-radius:var(--r4);padding:48px;text-align:center;cursor:pointer;transition:all .2s;background:var(--bg3)}
.import-zone:hover,.import-zone.dragover{border-color:var(--blue);background:var(--blue-l)}
.iz-icon{font-size:48px;margin-bottom:14px}.iz-title{font-size:16px;font-weight:700;margin-bottom:6px}.iz-sub{font-size:13px;color:var(--txt2);margin-bottom:14px}
.fmt-badge{padding:3px 10px;background:#fff;border:1px solid var(--bl);border-radius:8px;font-size:12px;font-weight:600;margin:0 4px}
.dv-hdr{background:#fff;border-bottom:1px solid var(--bl);padding:28px 36px}
.dv-hdr-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:20px}
.back-btn{display:inline-flex;align-items:center;gap:7px;padding:9px 14px;background:var(--bg3);border:1px solid var(--bl);border-radius:var(--r2);font-size:13.5px;font-weight:600;cursor:pointer;color:var(--txt2);transition:all .2s;font-family:inherit}
.back-btn:hover{background:var(--bl);color:var(--txt)}
.dv-name{font-family:'Fraunces',serif;font-size:30px;font-weight:700;margin-bottom:8px;display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.dv-info{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;background:var(--bg3);border-radius:var(--r3);padding:20px;margin-top:16px}
.dv-info-item{display:flex;flex-direction:column;gap:5px}
.dv-info-lbl{font-size:10px;text-transform:uppercase;letter-spacing:.5px;color:var(--txt3);font-weight:600}
.dv-info-val{font-size:13.5px;font-weight:600;color:var(--txt)}
.dv-body{padding:28px 36px;display:grid;grid-template-columns:1fr 380px;gap:28px}
.dv-main{display:flex;flex-direction:column;gap:20px}
.dv-panel{background:#fff;border-radius:var(--r4);border:1px solid var(--bl);overflow:hidden}
.dv-panel-hdr{padding:18px 22px;background:var(--bg3);border-bottom:1px solid var(--bl);font-size:15px;font-weight:700;display:flex;align-items:center;gap:10px}
.dv-panel-body{padding:22px}
.timeline{position:relative}
.timeline::before{content:'';position:absolute;left:14px;top:20px;bottom:20px;width:2px;background:var(--bl)}
.tl-item{display:flex;gap:18px;padding:14px 0;position:relative}
.tl-dot{width:30px;height:30px;background:#fff;border:3px solid var(--green);border-radius:50%;flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:13px;z-index:1}
.tl-content{flex:1;background:var(--bg3);padding:14px 18px;border-radius:var(--r2)}
.tl-date{font-size:11px;color:var(--txt3);font-weight:600;margin-bottom:5px}.tl-text{font-size:13.5px;color:var(--txt);line-height:1.5}
.dv-sidebar{display:flex;flex-direction:column;gap:20px}
.action-panel{background:#fff;border-radius:var(--r4);border:1px solid var(--bl);overflow:hidden}
.ap-hdr{padding:18px 22px;background:linear-gradient(135deg,var(--blue),var(--blue-d));color:#fff}
.ap-hdr h3{font-size:15px;font-weight:700;margin-bottom:3px}.ap-hdr p{font-size:11.5px;opacity:.8}
.ap-body{padding:22px}
.cur-action{padding:18px;background:var(--bg3);border-radius:var(--r3);margin-bottom:14px}
.ca-lbl{font-size:10px;text-transform:uppercase;letter-spacing:1px;color:var(--txt3);font-weight:700;margin-bottom:7px}
.ca-text{font-size:14.5px;font-weight:600;color:var(--txt);margin-bottom:7px}
.ca-date{font-size:12.5px;color:var(--blue);font-weight:600;display:flex;align-items:center;gap:5px}
.ca-time{font-size:12px;color:var(--purple);font-weight:600;margin-top:4px}
.qa-list{display:flex;flex-direction:column;gap:7px}
.qa-btn{width:100%;padding:12px 14px;border-radius:var(--r2);font-size:13.5px;font-weight:600;cursor:pointer;transition:all .2s;font-family:inherit;display:flex;align-items:center;gap:9px}
.qa-btn.complete{background:var(--green-l);border:2px solid var(--green);color:var(--green)}.qa-btn.complete:hover{background:var(--green);color:#fff}
.qa-btn.secondary{background:var(--bg3);border:1px solid var(--bl);color:var(--txt)}.qa-btn.secondary:hover{background:var(--bl)}
.detail-view{display:none}.detail-view.open{display:block}
.notif{position:fixed;bottom:28px;right:28px;display:none;align-items:center;gap:12px;padding:14px 20px;background:#fff;border-radius:var(--r3);box-shadow:var(--sh3);border-left:4px solid var(--green);z-index:9999;font-size:14px;font-weight:600}
.notif.error{border-left-color:var(--red)}.notif.warning{border-left-color:var(--orange)}
@media(max-width:1200px){.sidebar{width:230px}.main{margin-left:230px}.stats-bar{grid-template-columns:repeat(3,1fr)}.dv-body{grid-template-columns:1fr}}
@media(max-width:768px){.sidebar{display:none}.main{margin-left:0}.page-hdr,.stats-bar,.filters-bar,.page-content{padding-left:16px;padding-right:16px}.stats-bar{grid-template-columns:repeat(2,1fr)}.frow{grid-template-columns:1fr}.dv-info{grid-template-columns:repeat(2,1fr)}}
</style>
</head>
<body>
<div class="app">
<aside class="sidebar">
  <div class="sb-logo">
    <div class="sb-logo-row">
      <div class="sb-icon">◈</div>
      <div><div class="sb-title">SIPHRO</div><div class="sb-sub">Suivi Commercial</div></div>
    </div>
    <div class="sb-sync" id="sb-sync">🔄 Chargement...</div>
  </div>
  <nav class="sb-nav">
    <div class="sb-section">
      <div class="sb-sec-title">Navigation</div>
      <div class="nav-item active" id="nav-dashboard" onclick="showView('dashboard')"><span class="ni">📊</span>Tableau de bord</div>
      <div class="nav-item" id="nav-agenda" onclick="showView('agenda')"><span class="ni">📅</span>Agenda<span class="badge blue" id="sb-agenda-count">0</span></div>
    </div>
    <div class="sb-section">
      <div class="sb-sec-title">Catégories</div>
      <div class="nav-item" onclick="quickFilter('organisation')"><span class="dot8" style="background:#059669"></span><span>En organisation</span><span class="badge green" id="cnt-organisation">0</span></div>
      <div class="nav-item" onclick="quickFilter('negociation')"><span class="dot8" style="background:#2563eb"></span><span>En négociation</span><span class="badge blue" id="cnt-negociation">0</span></div>
      <div class="nav-item" onclick="quickFilter('chaud')"><span class="dot8" style="background:#d97706"></span><span>Dossiers chauds</span><span class="badge orange" id="cnt-chaud">0</span></div>
      <div class="nav-item" onclick="quickFilter('autre')"><span class="dot8" style="background:#7c3aed"></span><span>Autres à suivre</span><span class="badge" id="cnt-autre">0</span></div>
      <div class="nav-item" onclick="quickFilter('gagne')"><span class="dot8" style="background:#16a34a"></span><span>Dossiers gagnés</span><span class="badge green" id="cnt-gagne">0</span></div>
    </div>
    <div class="sb-section">
      <div class="sb-sec-title">Alertes</div>
      <div class="nav-item" onclick="quickFilter('overdue')" style="color:#ef4444"><span class="ni">⚠️</span>En retard<span class="badge" id="cnt-overdue">0</span></div>
      <div class="nav-item" onclick="quickFilter('today')" style="color:#f59e0b"><span class="ni">📅</span>Aujourd'hui<span class="badge orange" id="cnt-today">0</span></div>
      <div class="nav-item" onclick="quickFilter('week')" style="color:#3b82f6"><span class="ni">📆</span>Cette semaine<span class="badge blue" id="cnt-week">0</span></div>
    </div>
    <div class="sb-section">
      <div class="sb-sec-title">Données</div>
      <div class="nav-item" onclick="openImportModal()"><span class="ni">📥</span>Importer CSV/Excel</div>
      <div class="nav-item" onclick="exportData()"><span class="ni">📤</span>Exporter CSV</div>
    </div>
  </nav>
</aside>
<main class="main">

<!-- DASHBOARD VIEW -->
<div class="view active" id="view-dashboard">
  <header class="page-hdr">
    <div class="hdr-row">
      <div><h1 class="page-title">Tableau de bord</h1><p class="page-sub">Gérez vos prospects et relances commerciales</p></div>
      <div class="hdr-actions">
        <span class="sync-badge" id="sync-badge">🔄 Chargement...</span>
        <button class="btn btn-secondary" onclick="openImportModal()">📥 Importer</button>
        <button class="btn btn-primary" onclick="openProspectModal()">+ Nouveau prospect</button>
      </div>
    </div>
  </header>
  <div class="stats-bar">
    <div class="stat s-total" onclick="quickFilter('all')"><div class="stat-val" id="stat-total">0</div><div class="stat-lbl">Total Prospects</div><span class="stat-ico">📋</span></div>
    <div class="stat s-retard" onclick="quickFilter('overdue')"><div class="stat-val" id="stat-retard">0</div><div class="stat-lbl">En retard</div><span class="stat-ico">⚠️</span></div>
    <div class="stat s-today" onclick="quickFilter('today')"><div class="stat-val" id="stat-today">0</div><div class="stat-lbl">Aujourd'hui</div><span class="stat-ico">📅</span></div>
    <div class="stat s-nego" onclick="quickFilter('negociation')"><div class="stat-val" id="stat-nego">0</div><div class="stat-lbl">En négociation</div><span class="stat-ico">🤝</span></div>
    <div class="stat s-orga" onclick="quickFilter('organisation')"><div class="stat-val" id="stat-orga">0</div><div class="stat-lbl">En organisation</div><span class="stat-ico">📋</span></div>
  </div>
  <div class="filters-bar">
    <div class="search-wrap">
      <span class="si">🔍</span>
      <input type="text" id="search-input" placeholder="Rechercher nom, contact, formation, ville, email..." oninput="handleSearch(this.value)">
    </div>
    <div class="ftabs" id="filter-tabs">
      <button class="ftab active" onclick="setFilter('all')" data-f="all">Tous <span class="cnt" id="fc-all">0</span></button>
      <button class="ftab" onclick="setFilter('organisation')" data-f="organisation">📋 Orga <span class="cnt" id="fc-organisation">0</span></button>
      <button class="ftab" onclick="setFilter('negociation')" data-f="negociation">🤝 Négo <span class="cnt" id="fc-negociation">0</span></button>
      <button class="ftab" onclick="setFilter('chaud')" data-f="chaud">🔥 Chaud <span class="cnt" id="fc-chaud">0</span></button>
      <button class="ftab" onclick="setFilter('autre')" data-f="autre">📌 Autre <span class="cnt" id="fc-autre">0</span></button>
      <button class="ftab" onclick="setFilter('gagne')" data-f="gagne">✅ Gagné <span class="cnt" id="fc-gagne">0</span></button>
    </div>
    <div class="sort-wrap">
      <select id="sort-select" onchange="renderProspects()">
        <option value="urgency">Trier : Urgence</option>
        <option value="name">Trier : Nom A→Z</option>
        <option value="date-asc">Trier : Date ↑</option>
        <option value="date-desc">Trier : Date ↓</option>
        <option value="recent">Trier : Récemment ajouté</option>
      </select>
    </div>
    <div class="view-toggle">
      <button class="vt-btn active" id="vt-grid" onclick="setViewMode('grid')" title="Grille">⊞</button>
      <button class="vt-btn" id="vt-list" onclick="setViewMode('list')" title="Liste">☰</button>
    </div>
  </div>
  <div class="page-content">
    <div id="prospects-container"></div>
    <div class="empty" id="empty-state" style="display:none">
      <div class="empty-ico">📂</div>
      <h2 class="empty-title">Aucun prospect trouvé</h2>
      <p class="empty-sub">Ajoutez un nouveau prospect ou modifiez votre recherche</p>
      <div style="display:flex;gap:12px;justify-content:center">
        <button class="btn btn-secondary" onclick="openImportModal()">📥 Importer</button>
        <button class="btn btn-primary" onclick="openProspectModal()">+ Ajouter</button>
      </div>
    </div>
  </div>
  <div id="detail-view" class="detail-view"></div>
</div>

<!-- AGENDA VIEW -->
<div class="view" id="view-agenda">
  <header class="page-hdr">
    <div class="hdr-row">
      <div><h1 class="page-title">Agenda</h1><p class="page-sub">Planifiez vos actions par jour et tranche horaire</p></div>
      <div class="hdr-actions">
        <span class="sync-badge" id="sync-badge2">✅ Synchronisé</span>
        <button class="btn btn-primary" onclick="openActionModal(null,true)">+ Nouvelle tâche agenda</button>
      </div>
    </div>
  </header>
  <div class="agenda-nav">
    <div class="week-nav">
      <button onclick="agendaNav(-1)">← Préc</button>
      <div class="week-label" id="agenda-label">Semaine du…</div>
      <button onclick="agendaNav(1)">Suiv →</button>
      <button onclick="agendaGoToday()" style="margin-left:6px">Aujourd'hui</button>
    </div>
    <div class="at-toggle">
      <button class="at-btn" id="ag-list" onclick="setAgView('list')">📋 Liste</button>
      <button class="at-btn active" id="ag-week" onclick="setAgView('week')">📅 Semaine</button>
      <button class="at-btn" id="ag-day" onclick="setAgView('day')">🕐 Jour</button>
    </div>
  </div>
  <div class="agenda-container">
    <div id="agenda-content"></div>
  </div>
</div>

</main>
</div>

<!-- MODALS -->
<div class="modal-overlay" id="prospect-modal">
  <div class="modal">
    <div class="modal-hdr"><h2 class="modal-title" id="modal-title">Nouveau prospect</h2><button class="modal-close" onclick="closeProspectModal()">×</button></div>
    <div class="modal-body">
      <form id="prospect-form" onsubmit="handleProspectSubmit(event)">
        <input type="hidden" id="prospect-id">
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">🏢</span>Établissement</div>
          <div class="frow">
            <div class="fg"><label class="flbl">Nom établissement *</label><input type="text" class="finp" id="f-nom" required placeholder="Ex: Centre Hospitalier de Lyon"></div>
            <div class="fg"><label class="flbl">SIRET</label><input type="text" class="finp" id="f-siret" placeholder="14 chiffres"></div>
          </div>
          <div class="fg full"><label class="flbl">Adresse / Ville</label><input type="text" class="finp" id="f-adresse" placeholder="Adresse complète"></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">👤</span>Contact principal</div>
          <div class="frow">
            <div class="fg"><label class="flbl">Nom du contact</label><input type="text" class="finp" id="f-contact-nom" placeholder="Prénom Nom"></div>
            <div class="fg"><label class="flbl">Téléphone</label><input type="tel" class="finp" id="f-contact-tel" placeholder="06 XX XX XX XX"></div>
          </div>
          <div class="fg full"><label class="flbl">Email</label><input type="email" class="finp" id="f-contact-email" placeholder="email@exemple.fr"></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">📊</span>Classification</div>
          <div class="cat-sel" id="cat-sel">
            <div class="cat-opt" data-v="organisation" onclick="selectCat('organisation')"><span class="cat-dot" style="background:#059669"></span>📋 En organisation</div>
            <div class="cat-opt" data-v="negociation" onclick="selectCat('negociation')"><span class="cat-dot" style="background:#2563eb"></span>🤝 En négociation</div>
            <div class="cat-opt selected" data-v="chaud" onclick="selectCat('chaud')"><span class="cat-dot" style="background:#d97706"></span>🔥 Dossier chaud</div>
            <div class="cat-opt" data-v="autre" onclick="selectCat('autre')"><span class="cat-dot" style="background:#7c3aed"></span>📌 Autre à suivre</div>
            <div class="cat-opt" data-v="gagne" onclick="selectCat('gagne')"><span class="cat-dot" style="background:#16a34a"></span>✅ Dossier gagné</div>
          </div>
          <div class="fg" style="margin-top:12px"><label class="flbl">Formation visée</label><input type="text" class="finp" id="f-formation" placeholder="Ex: AFGSU 2, SST, Incendie..."></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">📝</span>Notes</div>
          <div class="fg full"><textarea class="ftxt" id="f-commentaire" rows="3" placeholder="Contexte, besoins spécifiques, observations..."></textarea></div>
        </div>
        <div class="fsec">
          <div class="fsec-title"><span class="fsec-ico">🎯</span>Prochaine action (optionnel)</div>
          <div class="frow">
            <div class="fg" style="grid-column:1/-1"><label class="flbl">Description</label><input type="text" class="finp" id="f-action-desc" placeholder="Ex: Appeler pour confirmer les dates"></div>
          </div>
          <div class="frow">
            <div class="fg"><label class="flbl">Date</label><input type="date" class="finp" id="f-action-date"></div>
            <div class="fg"><label class="flbl">Assigné à</label><input type="text" class="finp" id="f-action-assignee" placeholder="Collaborateur"></div>
          </div>
          <div class="frow">
            <div class="fg"><label class="flbl">Heure début</label><input type="time" class="finp" id="f-action-hstart"></div>
            <div class="fg"><label class="flbl">Heure fin</label><input type="time" class="finp" id="f-action-hend"></div>
          </div>
        </div>
      </form>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeProspectModal()">Annuler</button>
      <button class="btn btn-primary" onclick="document.getElementById('prospect-form').dispatchEvent(new Event('submit',{cancelable:true}))">✓ Enregistrer</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="action-modal">
  <div class="modal modal-sm">
    <div class="modal-hdr"><h2 class="modal-title" id="action-modal-title">Planifier une action</h2><button class="modal-close" onclick="closeActionModal()">×</button></div>
    <div class="modal-body">
      <form id="action-form" onsubmit="handleActionSubmit(event)">
        <input type="hidden" id="action-prospect-id">
        <div class="fg" style="margin-bottom:14px"><label class="flbl">Prospect *</label><input type="text" class="finp" id="action-prospect-search" placeholder="Tapez pour rechercher..." oninput="searchProspectInput(this.value)"><div id="prospect-dropdown" style="display:none;position:absolute;background:#fff;border:1px solid var(--bl);border-radius:var(--r2);box-shadow:var(--sh2);z-index:100;max-height:180px;overflow-y:auto;width:400px"></div></div>
        <div class="fg" style="margin-bottom:16px"><label class="flbl">Description *</label><input type="text" class="finp" id="action-desc" required placeholder="Ex: Appeler pour suivre le devis"></div>
        <div class="frow">
          <div class="fg"><label class="flbl">Date *</label><input type="date" class="finp" id="action-date" required></div>
          <div class="fg"><label class="flbl">Assigné à</label><input type="text" class="finp" id="action-assignee" placeholder="Collaborateur"></div>
        </div>
        <div class="frow">
          <div class="fg"><label class="flbl">Heure début</label><input type="time" class="finp" id="action-hstart"></div>
          <div class="fg"><label class="flbl">Heure fin</label><input type="time" class="finp" id="action-hend"></div>
        </div>
        <div class="fg" style="margin-top:12px"><label class="flbl">Notes</label><textarea class="ftxt" id="action-notes" rows="2" placeholder="Détails supplémentaires..."></textarea></div>
      </form>
    </div>
    <div class="modal-ftr">
      <button class="btn btn-secondary" onclick="closeActionModal()">Annuler</button>
      <button class="btn btn-primary" onclick="document.getElementById('action-form').dispatchEvent(new Event('submit',{cancelable:true}))">✓ Planifier</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="import-modal">
  <div class="modal">
    <div class="modal-hdr"><h2 class="modal-title">Importer des contacts</h2><button class="modal-close" onclick="closeImportModal()">×</button></div>
    <div class="modal-body">
      <div class="import-zone" id="import-zone" onclick="document.getElementById('file-input').click()">
        <div class="iz-icon">📁</div>
        <div class="iz-title">Glissez votre fichier ici</div>
        <div class="iz-sub">ou cliquez pour sélectionner</div>
        <div><span class="fmt-badge">CSV</span><span class="fmt-badge">Excel (.xlsx)</span></div>
      </div>
      <input type="file" id="file-input" accept=".csv,.xlsx,.xls" style="display:none" onchange="handleFileSelect(event)">
      <div style="margin-top:20px;padding:18px;background:var(--bg3);border-radius:var(--r3)">
        <h4 style="font-size:13.5px;font-weight:700;margin-bottom:10px">📋 Colonnes attendues</h4>
        <div style="background:#fff;padding:10px;border-radius:var(--r1);font-family:monospace;font-size:11px;overflow-x:auto;white-space:nowrap">nom_etablissement ; contact_nom ; contact_telephone ; contact_email ; adresse ; siret ; categorie ; formation_visee ; commentaire ; prochaine_action ; date_prochaine_action</div>
        <p style="font-size:11.5px;color:var(--txt3);margin-top:10px"><strong>Catégories :</strong> organisation, negociation, chaud, autre, gagne</p>
      </div>
    </div>
    <div class="modal-ftr"><button class="btn btn-secondary" onclick="closeImportModal()">Fermer</button></div>
  </div>
</div>

<div class="notif" id="notif" style="display:none">
  <span id="notif-ico">✓</span>
  <span id="notif-text">Action effectuée</span>
</div>
<script>
// ===== CONSTANTS =====
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbyn_49XC_MOUfCt714IP_oSxhGNGUn9n1M6xm5AsdHiWKN-XRdeS8cCQAcSuG8v0dXMnA/exec';
const CAT_COLORS={organisation:'#059669',negociation:'#2563eb',chaud:'#d97706',autre:'#7c3aed',gagne:'#16a34a'};
const CAT_LABELS={organisation:{l:'Organisation',i:'📋'},negociation:{l:'Négociation',i:'🤝'},chaud:{l:'Dossier chaud',i:'🔥'},autre:{l:'À suivre',i:'📌'},gagne:{l:'Gagné',i:'✅'}};
const HOURS=['08:00','09:00','10:00','11:00','12:00','13:00','14:00','15:00','16:00','17:00','18:00','19:00'];
const DAYS_FR=['Dim','Lun','Mar','Mer','Jeu','Ven','Sam'];
const MONTHS_FR=['janvier','février','mars','avril','mai','juin','juillet','août','septembre','octobre','novembre','décembre'];

// ===== STATE =====
let prospects=[];
let currentFilter='all';
let filterType='category';
let searchQuery='';
let selectedCat='chaud';
let viewMode='grid';
let currentView='dashboard';
let agView='week';
let agOffset=0;
let agDayOffset=0;
let isSaving=false;
let syncInterval=null;
let agendaQuickDate='';
let agendaQuickHour='';

// ===== GOOGLE SHEETS STORAGE =====
function setSyncStatus(msg,color){
  ['sb-sync','sync-badge','sync-badge2'].forEach(id=>{
    const e=document.getElementById(id);
    if(e){e.textContent=msg;if(color)e.style.color=color;}
  });
}

async function loadData(){
  setSyncStatus('🔄 Chargement...','#d97706');
  try {
    const res = await fetch(GOOGLE_SCRIPT_URL);
    const raw = await res.json();
    // Chaque ligne du sheet devient un prospect
    prospects = raw.map(row => {
      let prochaineAction = null;
      let actionsEffectuees = [];
      try { prochaineAction = row.prochaineAction ? JSON.parse(row.prochaineAction) : null; } catch(e){}
      try { actionsEffectuees = row.actionsEffectuees ? JSON.parse(row.actionsEffectuees) : []; } catch(e){}
      return {
        id: row.id || generateId(),
        nom: row.nom || '',
        contactNom: row.contactNom || '',
        contactTel: row.contactTel || '',
        contactEmail: row.contactEmail || '',
        adresse: row.adresse || '',
        siret: row.siret || '',
        categorie: row.categorie || 'chaud',
        formation: row.formation || '',
        commentaire: row.commentaire || '',
        prochaineAction: prochaineAction,
        actionsEffectuees: actionsEffectuees,
        createdAt: row.createdAt || new Date().toISOString()
      };
    }).filter(p => p.nom).map(enrichProspect); // ignore les lignes vides + enrichit téléphone & horaire
    setSyncStatus('✅ Synchronisé — Google Sheets','#059669');
  } catch(e) {
    setSyncStatus('❌ Impossible de charger les données','#dc2626');
    showNotif('Erreur de connexion Google Sheets — vérifiez votre connexion internet','error');
    prospects = [];
  }
  renderAll();
  // Auto-refresh toutes les 20 secondes
  if(!syncInterval) syncInterval = setInterval(refreshData, 20000);
}

async function refreshData(){
  try {
    const res = await fetch(GOOGLE_SCRIPT_URL + '?t=' + Date.now());
    const raw = await res.json();
    const remote = raw.map(row => {
      let prochaineAction = null;
      let actionsEffectuees = [];
      try { prochaineAction = row.prochaineAction ? JSON.parse(row.prochaineAction) : null; } catch(e){}
      try { actionsEffectuees = row.actionsEffectuees ? JSON.parse(row.actionsEffectuees) : []; } catch(e){}
      return {
        id: row.id || '',
        nom: row.nom || '',
        contactNom: row.contactNom || '',
        contactTel: row.contactTel || '',
        contactEmail: row.contactEmail || '',
        adresse: row.adresse || '',
        siret: row.siret || '',
        categorie: row.categorie || 'chaud',
        formation: row.formation || '',
        commentaire: row.commentaire || '',
        prochaineAction: prochaineAction,
        actionsEffectuees: actionsEffectuees,
        createdAt: row.createdAt || ''
      };
    }).filter(p => p.nom).map(enrichProspect);
    if(JSON.stringify(remote) !== JSON.stringify(prospects)){
      prospects = remote;
      renderAll();
      setSyncStatus('🔄 Mis à jour','#2563eb');
      setTimeout(()=>setSyncStatus('✅ Synchronisé — Google Sheets','#059669'), 2000);
    }
  } catch(e) { /* silencieux */ }
}

async function saveData(){
  if(isSaving) return;
  isSaving = true;
  setSyncStatus('💾 Sauvegarde dans Google Sheets...','#d97706');
  try {
    await fetch(GOOGLE_SCRIPT_URL, {
      method: 'POST',
      mode: 'no-cors',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ action: 'save_all', data: prospects })
    });
    // Avec no-cors on ne peut pas lire la réponse, mais ça fonctionne
    setSyncStatus('✅ Sauvegardé — Google Sheets','#059669');
    showNotif('Données sauvegardées dans Google Sheets ✅');
  } catch(e) {
    setSyncStatus('❌ Erreur de sauvegarde','#dc2626');
    showNotif('Erreur de sauvegarde','error');
  }
  isSaving = false;
  renderAll();
}

function generateId(){return Date.now().toString(36)+Math.random().toString(36).substr(2);}

// ===== FORMAT TELEPHONE =====
function formatTel(raw){
  // Google Sheets renvoie parfois des nombres entiers (ex: 467867673) au lieu de strings
  if(raw===null||raw===undefined)return '';
  raw=String(raw).trim();
  if(raw===''||raw==='-'||raw==='—')return raw;
  // Doubles numéros séparés par / ou |
  if(raw.includes('/')||raw.includes('|'))return raw.split(/[\/|]/).map(p=>formatTel(p.trim())).join(' / ');
  let digits=raw.replace(/\D/g,'');
  if(!digits)return raw;
  // Supprimer indicatif +33 ou 0033
  if(digits.startsWith('330')&&digits.length===11)digits='0'+digits.slice(2);
  else if(digits.startsWith('33')&&digits.length===11)digits='0'+digits.slice(2);
  // Compléter si 9 chiffres (manque le 0 initial — cas fréquent depuis Sheets)
  if(digits.length===9)digits='0'+digits;
  // Formater en XX XX XX XX XX
  if(digits.length===10)return digits.replace(/(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/,'$1 $2 $3 $4 $5');
  return raw;
}

// ===== HORAIRE ALEATOIRE DETERMINISTE =====
const SLOT_HOURS=["09:00","09:30","10:00","10:30","11:00","11:30","14:00","14:30","15:00","15:30","16:00","16:30","17:00","17:30"];
function randomSlot(seed){
  const h=Math.abs((seed||"x").split("").reduce((a,c)=>a+c.charCodeAt(0),0))%SLOT_HOURS.length;
  return{hstart:SLOT_HOURS[h],hend:SLOT_HOURS[Math.min(h+1,SLOT_HOURS.length-1)]};
}
function enrichProspect(p){
  p.contactTel=formatTel(p.contactTel);
  if(p.prochaineAction&&!p.prochaineAction.hstart){
    const slot=randomSlot(p.id||p.nom);
    p.prochaineAction.hstart=slot.hstart;
    p.prochaineAction.hend=slot.hend;
  }
  return p;
}

// ===== RENDER ALL =====
function renderAll(){renderStats();renderSidebarCounts();renderProspects();if(currentView==='agenda')renderAgenda();}

function today0(){const d=new Date();d.setHours(0,0,0,0);return d;}

function renderStats(){
  const now=today0();
  document.getElementById('stat-total').textContent=prospects.length;
  document.getElementById('stat-retard').textContent=prospects.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now).length;
  document.getElementById('stat-today').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();}).length;
  document.getElementById('stat-nego').textContent=prospects.filter(p=>p.categorie==='negociation').length;
  document.getElementById('stat-orga').textContent=prospects.filter(p=>p.categorie==='organisation').length;
}
function renderSidebarCounts(){
  const now=today0();const we=new Date(now);we.setDate(we.getDate()+7);
  ['organisation','negociation','chaud','autre','gagne'].forEach(c=>{document.getElementById('cnt-'+c).textContent=prospects.filter(p=>p.categorie===c).length;});
  document.getElementById('cnt-overdue').textContent=prospects.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now).length;
  document.getElementById('cnt-today').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();}).length;
  document.getElementById('cnt-week').textContent=prospects.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);return d>=now&&d<=we;}).length;
  document.getElementById('sb-agenda-count').textContent=prospects.filter(p=>p.prochaineAction).length;
  ['all','organisation','negociation','chaud','autre','gagne'].forEach(f=>{const el=document.getElementById('fc-'+f);if(el)el.textContent=f==='all'?prospects.length:prospects.filter(p=>p.categorie===f).length;});
}

// ===== FILTER =====
function setFilter(f){
  currentFilter=f;filterType='category';
  document.querySelectorAll('.ftab').forEach(t=>t.classList.toggle('active',t.dataset.f===f));
  renderProspects();
}
function quickFilter(f){
  showView('dashboard');
  if(['overdue','today','week'].includes(f)){filterType='urgency';currentFilter=f;document.querySelectorAll('.ftab').forEach(t=>t.classList.remove('active'));}
  else{setFilter(f);}
  renderProspects();
}
function handleSearch(v){searchQuery=v;renderProspects();}
function getFiltered(){
  let list=[...prospects];
  const now=today0();const we=new Date(now);we.setDate(we.getDate()+7);
  if(filterType==='urgency'){
    if(currentFilter==='overdue')list=list.filter(p=>p.prochaineAction&&new Date(p.prochaineAction.date)<now);
    else if(currentFilter==='today')list=list.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return d.getTime()===now.getTime();});
    else if(currentFilter==='week')list=list.filter(p=>{if(!p.prochaineAction)return false;const d=new Date(p.prochaineAction.date);return d>=now&&d<=we;});
  }else{if(currentFilter!=='all')list=list.filter(p=>p.categorie===currentFilter);}
  if(searchQuery){
    const q=searchQuery.toLowerCase();
    list=list.filter(p=>(p.nom||'').toLowerCase().includes(q)||(p.formation||'').toLowerCase().includes(q)||(p.contactNom||'').toLowerCase().includes(q)||(p.commentaire||'').toLowerCase().includes(q)||(p.adresse||'').toLowerCase().includes(q)||(p.contactTel||'').toLowerCase().includes(q)||(p.contactEmail||'').toLowerCase().includes(q)||(p.siret||'').toLowerCase().includes(q));
  }
  const sort=document.getElementById('sort-select')?.value||'urgency';
  if(sort==='urgency')list.sort((a,b)=>{const sc=p=>{if(!p.prochaineAction)return 9999;const d=new Date(p.prochaineAction.date);d.setHours(0,0,0,0);return(d-now)/86400000;};return sc(a)-sc(b);});
  else if(sort==='name')list.sort((a,b)=>a.nom.localeCompare(b.nom));
  else if(sort==='date-asc')list.sort((a,b)=>{const da=a.prochaineAction?new Date(a.prochaineAction.date):new Date(9999,0);const db=b.prochaineAction?new Date(b.prochaineAction.date):new Date(9999,0);return da-db;});
  else if(sort==='date-desc')list.sort((a,b)=>{const da=a.prochaineAction?new Date(a.prochaineAction.date):new Date(0);const db=b.prochaineAction?new Date(b.prochaineAction.date):new Date(0);return db-da;});
  else if(sort==='recent')list.sort((a,b)=>new Date(b.createdAt)-new Date(a.createdAt));
  return list;
}
function setViewMode(m){viewMode=m;document.getElementById('vt-grid').classList.toggle('active',m==='grid');document.getElementById('vt-list').classList.toggle('active',m==='list');renderProspects();}

// ===== RENDER PROSPECTS =====
function renderProspects(){
  const container=document.getElementById('prospects-container');
  const empty=document.getElementById('empty-state');
  const filtered=getFiltered();
  if(filtered.length===0){container.innerHTML='';empty.style.display='block';}
  else{
    empty.style.display='none';
    if(viewMode==='list'){container.innerHTML='<div class="prospects-list">'+filtered.map(renderListRow).join('')+'</div>';}
    else{container.innerHTML='<div class="prospects-grid">'+filtered.map(renderCard).join('')+'</div>';}
  }
}

// ===== URGENCY =====
function getUrgency(action){
  if(!action)return{cls:'none',lbl:'',ico:''};
  const now=today0(),d=new Date(action.date);d.setHours(0,0,0,0);
  const diff=(d-now)/86400000;
  if(diff<0)return{cls:'overdue',lbl:'EN RETARD',ico:'⚠️'};
  if(diff===0)return{cls:'today',lbl:"AUJOURD'HUI",ico:'📅'};
  if(diff<=3)return{cls:'soon',lbl:'BIENTÔT',ico:'⏰'};
  return{cls:'normal',lbl:'À FAIRE',ico:'📌'};
}

// ===== ESC =====
function esc(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}

// ===== FORMAT DATES =====
function fmtDate(ds){return new Date(ds).toLocaleDateString('fr-FR',{day:'2-digit',month:'2-digit',year:'numeric'});}
function fmtDT(ds){return new Date(ds).toLocaleString('fr-FR',{day:'2-digit',month:'2-digit',year:'numeric',hour:'2-digit',minute:'2-digit'});}
function fmtDayFull(d){return DAYS_FR[d.getDay()]+' '+d.getDate()+' '+MONTHS_FR[d.getMonth()]+' '+d.getFullYear();}

// ===== CARD =====
function renderCard(p){
  const cat=CAT_LABELS[p.categorie]||CAT_LABELS.autre;
  const urg=getUrgency(p.prochaineAction);
  const hc=p.actionsEffectuees?.length||0;
  return`<div class="card" onclick="openDetailView('${p.id}')">
<div class="card-hdr">
  <div>
    <div class="card-name">${esc(p.nom)}</div>
    <span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span>
    ${p.formation?`<div class="card-formation">🎓 ${esc(p.formation)}</div>`:''}
  </div>
  <div class="card-acts" onclick="event.stopPropagation()">
    <button class="ibt" onclick="editProspect('${p.id}')" title="Modifier">✏️</button>
    <button class="ibt del" onclick="deleteProspect('${p.id}')" title="Supprimer">🗑️</button>
  </div>
</div>
<div class="card-body">
  <div class="ci-grid">
    ${p.contactNom?`<div class="ci"><span class="ico">👤</span><span>${esc(p.contactNom)}</span></div>`:''}
    ${p.contactTel?`<div class="ci"><span class="ico">📞</span><span>${esc(p.contactTel)}</span></div>`:''}
    ${p.contactEmail?`<div class="ci"><span class="ico">✉️</span><span>${esc(p.contactEmail)}</span></div>`:''}
    ${p.adresse?`<div class="ci"><span class="ico">📍</span><span>${esc(p.adresse)}</span></div>`:''}
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
    <p class="adesc">${esc(p.prochaineAction.description)}${p.prochaineAction.assignee?`<br><small style="color:var(--txt3)">👤 ${esc(p.prochaineAction.assignee)}</small>`:''}</p>
    <button class="acomplete" onclick="event.stopPropagation();completeAction('${p.id}')">✓ Marquer effectuée</button>
  </div>`:`
  <div class="no-action"><button class="add-action-btn" onclick="event.stopPropagation();openActionModal('${p.id}')">+ Planifier une action</button></div>`}
  ${hc>0?`
  <div class="hist-section">
    <button class="hist-toggle" onclick="event.stopPropagation();toggleHist(this)"><span class="arr">▶</span> Historique (${hc} action${hc>1?'s':''})</button>
    <div class="hist-list">${p.actionsEffectuees.slice(-3).reverse().map(a=>`<div class="hist-item"><span class="hdot"></span><div><div class="hdate">${fmtDT(a.completedAt)}</div><div class="htext">${esc(a.description)}</div></div></div>`).join('')}</div>
  </div>`:''}
</div></div>`;
}

function renderListRow(p){
  const cat=CAT_LABELS[p.categorie]||CAT_LABELS.autre;
  const urg=getUrgency(p.prochaineAction);
  const col=urg.cls==='overdue'?'var(--red)':urg.cls==='today'?'var(--orange)':'var(--blue)';
  return`<div class="list-row" onclick="openDetailView('${p.id}')">
<div class="lr-name">${esc(p.nom)}${p.formation?`<br><small style="color:var(--txt3);font-weight:400">🎓 ${esc(p.formation)}</small>`:''}</div>
<div><span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span></div>
<div style="font-size:13px;color:var(--txt2)">${p.contactNom?esc(p.contactNom):'—'}<br><span style="font-size:11px;color:var(--txt3)">${p.contactTel||''}</span></div>
<div style="font-size:12.5px">${p.prochaineAction?esc(p.prochaineAction.description):'<span style="color:var(--txt3)">Aucune action</span>'}</div>
<div>${p.prochaineAction?`<span style="font-size:12px;color:${col};font-weight:700">${urg.ico} ${fmtDate(p.prochaineAction.date)}</span>`:'—'}</div>
<div class="card-acts" onclick="event.stopPropagation()">
  <button class="ibt" onclick="editProspect('${p.id}')">✏️</button>
  ${p.prochaineAction?`<button class="ibt" onclick="completeAction('${p.id}')" title="Effectuée">✅</button>`:''}
  <button class="ibt del" onclick="deleteProspect('${p.id}')">🗑️</button>
</div></div>`;
}
function toggleHist(btn){btn.classList.toggle('open');btn.nextElementSibling.classList.toggle('open');}

// ===== VIEWS =====
function showView(v){
  currentView=v;
  document.querySelectorAll('.view').forEach(el=>el.classList.remove('active'));
  document.getElementById('view-'+v).classList.add('active');
  document.querySelectorAll('.nav-item').forEach(ni=>ni.classList.remove('active'));
  document.getElementById('nav-'+v)?.classList.add('active');
  if(v==='agenda')renderAgenda();
}

// ===== AGENDA =====
function agendaNav(dir){if(agView==='day')agDayOffset+=dir;else agOffset+=dir;renderAgenda();}
function agendaGoToday(){agOffset=0;agDayOffset=0;renderAgenda();}
function setAgView(v){agView=v;['list','week','day'].forEach(x=>document.getElementById('ag-'+x)?.classList.toggle('active',x===v));renderAgenda();}

function getWeekStart(off){
  const d=new Date();const day=d.getDay();const diff=d.getDate()-(day===0?6:day-1)+off*7;d.setDate(diff);d.setHours(0,0,0,0);return d;
}
function getActionsForDay(date){
  const d0=new Date(date);d0.setHours(0,0,0,0);
  return prospects.filter(p=>{if(!p.prochaineAction)return false;const pd=new Date(p.prochaineAction.date);pd.setHours(0,0,0,0);return pd.getTime()===d0.getTime();}).map(p=>({...p.prochaineAction,prospectId:p.id,prospectNom:p.nom,categorie:p.categorie}));
}

function renderAgenda(){
  const c=document.getElementById('agenda-content');if(!c)return;
  if(agView==='week')renderWeekGrid(c);
  else if(agView==='day')renderDayGrid(c);
  else renderAgendaList(c);
}

function renderWeekGrid(c){
  const ws=getWeekStart(agOffset);const we=new Date(ws);we.setDate(we.getDate()+6);
  document.getElementById('agenda-label').textContent=`Semaine du ${ws.getDate()} au ${we.getDate()} ${MONTHS_FR[we.getMonth()]} ${we.getFullYear()}`;
  const days=Array.from({length:7},(_,i)=>{const d=new Date(ws);d.setDate(ws.getDate()+i);return d;});
  const ts=today0().toDateString();
  let html=`<div class="week-grid"><div class="wg-corner"></div>`;
  days.forEach(d=>{
    const iT=d.toDateString()===ts;
    html+=`<div class="wg-day-hdr${iT?' today-col':''}"><div class="wg-day-name">${DAYS_FR[d.getDay()]}</div><div class="wg-day-num">${d.getDate()}</div></div>`;
  });
  HOURS.forEach(h=>{
    const hn=parseInt(h);
    html+=`<div class="wg-time">${h}</div>`;
    days.forEach(d=>{
      const iT=d.toDateString()===ts;
      const acts=getActionsForDay(d).filter(a=>a.hstart&&parseInt(a.hstart)===hn);
      html+=`<div class="wg-cell${iT?' today-col':''}">`;
      acts.forEach(a=>{const urg=getUrgency({date:a.date});html+=`<div class="wg-event ${urg.cls}" onclick="openDetailView('${a.prospectId}')" title="${esc(a.prospectNom)} — ${esc(a.description)}">${esc(a.prospectNom)}</div>`;});
      if(!acts.length)html+=`<div class="wg-add" onclick="quickAddAgenda('${d.toISOString().split('T')[0]}','${h}')">+</div>`;
      html+='</div>';
    });
  });
  html+='</div>';
  c.innerHTML=`<div class="agenda-grid">${html}</div>`;
}

function renderDayGrid(c){
  const d=new Date();d.setHours(0,0,0,0);d.setDate(d.getDate()+agDayOffset);
  const ts=today0().toDateString();const iT=d.toDateString()===ts;
  document.getElementById('agenda-label').textContent=`${DAYS_FR[d.getDay()]} ${d.getDate()} ${MONTHS_FR[d.getMonth()]} ${d.getFullYear()}${iT?' — Aujourd\'hui':''}`;
  const acts=getActionsForDay(d);
  let html=`<div class="day-grid">`;
  const noTimeActs=acts.filter(a=>!a.hstart);
  HOURS.forEach((h,hi)=>{
    const hn=parseInt(h);
    const slotActs=acts.filter(a=>a.hstart&&parseInt(a.hstart)===hn);
    html+=`<div class="day-time">${h}</div><div class="day-slot">`;
    if(hi===0&&noTimeActs.length){
      noTimeActs.forEach(a=>{const urg=getUrgency({date:a.date});html+=`<div class="day-event ${urg.cls}" onclick="openDetailView('${a.prospectId}')"><span style="opacity:.7">⏰ libre</span><span style="flex:1">${esc(a.prospectNom)} — ${esc(a.description)}</span><button class="cpl-btn" onclick="event.stopPropagation();completeAction('${a.prospectId}')" style="padding:3px 8px;font-size:11px">✓</button></div>`;});
    }
    slotActs.forEach(a=>{const urg=getUrgency({date:a.date});html+=`<div class="day-event ${urg.cls}" onclick="openDetailView('${a.prospectId}')"><span>${a.hstart}${a.hend?' – '+a.hend:''}</span><span style="flex:1">${esc(a.prospectNom)} — ${esc(a.description)}</span><button class="cpl-btn" onclick="event.stopPropagation();completeAction('${a.prospectId}')" style="padding:3px 8px;font-size:11px">✓</button></div>`;});
    if(!slotActs.length&&!(hi===0&&noTimeActs.length))html+=`<div class="wg-add" onclick="quickAddAgenda('${d.toISOString().split('T')[0]}','${h}')">+</div>`;
    html+='</div>';
  });
  html+='</div>';
  c.innerHTML=`<div class="agenda-grid">${html}</div>`;
}

function renderAgendaList(c){
  const now=today0();
  const all=prospects.filter(p=>p.prochaineAction).sort((a,b)=>new Date(a.prochaineAction.date)-new Date(b.prochaineAction.date));
  if(!all.length){c.innerHTML='<div class="agenda-list-wrap"><div class="empty"><div class="empty-ico">📅</div><h2 class="empty-title">Aucune action planifiée</h2></div></div>';return;}
  const groups={};
  all.forEach(p=>{const dk=p.prochaineAction.date;if(!groups[dk])groups[dk]=[];groups[dk].push(p);});
  let html='<div class="agenda-list-wrap">';
  Object.keys(groups).sort().forEach(dk=>{
    const d=new Date(dk);const isT=d.toDateString()===now.toDateString();const isP=d<now;
    html+=`<div class="adg"><div class="adg-title" style="${isP?'color:var(--red)':isT?'color:var(--orange)':''}">📅 ${fmtDayFull(d)} ${isT?`<span class="adg-tag" style="background:var(--orange)">AUJOURD'HUI</span>`:''}${isP?`<span class="adg-tag" style="background:var(--red)">EN RETARD</span>`:''}</div>`;
    groups[dk].forEach(p=>{
      const a=p.prochaineAction;const urg=getUrgency(a);
      html+=`<div class="ai" onclick="openDetailView('${p.id}')">
<div class="ai-time">${a.hstart||'—'}</div>
<div class="ai-dot" style="background:${CAT_COLORS[p.categorie]||'#64748b'}"></div>
<div style="flex:1"><div class="ai-prospect">${esc(p.nom)}</div><div class="ai-action">${esc(a.description)}${a.assignee?` · 👤 ${esc(a.assignee)}`:''}</div></div>
<span class="cbadge ${p.categorie}">${(CAT_LABELS[p.categorie]||CAT_LABELS.autre).i}</span>
<button class="cpl-btn" onclick="event.stopPropagation();completeAction('${p.id}')">✓ Effectuée</button>
</div>`;
    });
    html+='</div>';
  });
  html+='</div>';c.innerHTML=html;
}

function quickAddAgenda(ds,hs){
  agendaQuickDate=ds;agendaQuickHour=hs;
  openActionModal(null,true,ds,hs);
}

// ===== DETAIL VIEW =====
function openDetailView(id){
  const p=prospects.find(x=>x.id===id);if(!p)return;
  const cat=CAT_LABELS[p.categorie]||CAT_LABELS.autre;
  const urg=getUrgency(p.prochaineAction);
  const hc=p.actionsEffectuees?.length||0;
  const dv=document.getElementById('detail-view');
  dv.innerHTML=`
<div class="dv-hdr">
  <div class="dv-hdr-top">
    <button class="back-btn" onclick="closeDV()">← Retour</button>
    <div class="hdr-actions">
      <button class="btn btn-secondary btn-sm" onclick="editProspect('${p.id}')">✏️ Modifier</button>
      <button class="btn btn-danger btn-sm" onclick="deleteProspect('${p.id}')">🗑️ Supprimer</button>
    </div>
  </div>
  <div class="dv-name">${esc(p.nom)} <span class="cbadge ${p.categorie}">${cat.i} ${cat.l}</span></div>
  <div class="dv-info">
    <div class="dv-info-item"><span class="dv-info-lbl">Contact</span><span class="dv-info-val">${esc(p.contactNom||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Téléphone</span><span class="dv-info-val">${esc(p.contactTel||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Email</span><span class="dv-info-val" style="font-size:12.5px">${esc(p.contactEmail||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Formation</span><span class="dv-info-val">${esc(p.formation||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">SIRET</span><span class="dv-info-val">${esc(p.siret||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Adresse</span><span class="dv-info-val" style="font-size:12.5px">${esc(p.adresse||'—')}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Créé le</span><span class="dv-info-val">${fmtDate(p.createdAt)}</span></div>
    <div class="dv-info-item"><span class="dv-info-lbl">Actions effectuées</span><span class="dv-info-val">${hc}</span></div>
  </div>
</div>
<div class="dv-body">
  <div class="dv-main">
    ${p.commentaire?`<div class="dv-panel"><div class="dv-panel-hdr">📝 Notes</div><div class="dv-panel-body"><p style="line-height:1.7;color:var(--txt2)">${esc(p.commentaire)}</p></div></div>`:''}
    <div class="dv-panel">
      <div class="dv-panel-hdr">📜 Historique des actions</div>
      <div class="dv-panel-body">
        ${hc>0?`<div class="timeline">${p.actionsEffectuees.slice().reverse().map(a=>`<div class="tl-item"><div class="tl-dot">✓</div><div class="tl-content"><div class="tl-date">${fmtDT(a.completedAt)}</div><div class="tl-text">${esc(a.description)}</div></div></div>`).join('')}</div>`:'<p style="color:var(--txt3);text-align:center;padding:28px 0">Aucune action effectuée</p>'}
      </div>
    </div>
  </div>
  <div class="dv-sidebar">
    <div class="action-panel">
      <div class="ap-hdr"><h3>🎯 Prochaine action</h3><p>Suivi commercial</p></div>
      <div class="ap-body">
        ${p.prochaineAction?`
        <div class="cur-action" style="border-left:4px solid ${urg.cls==='overdue'?'var(--red)':urg.cls==='today'?'var(--orange)':'var(--blue)'}">
          <div class="ca-lbl">${urg.ico} ${urg.lbl}</div>
          <div class="ca-text">${esc(p.prochaineAction.description)}</div>
          <div class="ca-date">📅 ${fmtDate(p.prochaineAction.date)}</div>
          ${p.prochaineAction.hstart?`<div class="ca-time">⏰ ${p.prochaineAction.hstart}${p.prochaineAction.hend?' – '+p.prochaineAction.hend:''}</div>`:''}
          ${p.prochaineAction.assignee?`<div style="font-size:12px;color:var(--txt3);margin-top:4px">👤 ${esc(p.prochaineAction.assignee)}</div>`:''}
        </div>
        <div class="qa-list">
          <button class="qa-btn complete" onclick="completeAction('${p.id}')">✓ Marquer comme effectuée</button>
          <button class="qa-btn secondary" onclick="openActionModal('${p.id}')">📅 Modifier l'action</button>
        </div>`:`
        <div style="text-align:center;padding:24px 0">
          <p style="color:var(--txt3);margin-bottom:16px">Aucune action planifiée</p>
          <button class="btn btn-primary btn-sm" onclick="openActionModal('${p.id}')">+ Planifier une action</button>
        </div>`}
      </div>
    </div>
    <div class="dv-panel">
      <div class="dv-panel-hdr">⚡ Actions rapides</div>
      <div class="dv-panel-body">
        <div class="qa-list">
          <button class="qa-btn secondary" onclick="changeCat('${p.id}','organisation')">📋 Passer en organisation</button>
          <button class="qa-btn secondary" onclick="changeCat('${p.id}','negociation')">🤝 Passer en négociation</button>
          <button class="qa-btn secondary" onclick="changeCat('${p.id}','chaud')">🔥 Marquer comme chaud</button>
          <button class="qa-btn secondary" onclick="changeCat('${p.id}','gagne')">✅ Marquer comme gagné</button>
        </div>
      </div>
    </div>
  </div>
</div>`;
  document.getElementById('dashboard-view-content')&&(document.getElementById('dashboard-view-content').style.display='none');
  dv.classList.add('open');
  document.querySelector('#view-dashboard .page-hdr')&&(document.querySelector('#view-dashboard .page-hdr').style.display='none');
  document.querySelector('.stats-bar')&&(document.querySelector('.stats-bar').style.display='none');
  document.querySelector('.filters-bar')&&(document.querySelector('.filters-bar').style.display='none');
  document.querySelector('.page-content')&&(document.querySelector('.page-content').style.display='none');
}
function closeDV(){
  document.getElementById('detail-view').classList.remove('open');
  document.querySelector('#view-dashboard .page-hdr')&&(document.querySelector('#view-dashboard .page-hdr').style.display='');
  document.querySelector('.stats-bar')&&(document.querySelector('.stats-bar').style.display='');
  document.querySelector('.filters-bar')&&(document.querySelector('.filters-bar').style.display='');
  document.querySelector('.page-content')&&(document.querySelector('.page-content').style.display='');
  renderProspects();
}
function changeCat(id,cat){
  const i=prospects.findIndex(p=>p.id===id);
  if(i!==-1){prospects[i].categorie=cat;saveData();openDetailView(id);showNotif('Catégorie mise à jour');}
}

// ===== CRUD =====
function openProspectModal(id=null){
  const modal=document.getElementById('prospect-modal');
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
  }
  modal.classList.add('open');
}
function closeProspectModal(){document.getElementById('prospect-modal').classList.remove('open');}

function handleProspectSubmit(e){
  e.preventDefault();
  const id=document.getElementById('prospect-id').value;
  const data={nom:document.getElementById('f-nom').value,siret:document.getElementById('f-siret').value,adresse:document.getElementById('f-adresse').value,contactNom:document.getElementById('f-contact-nom').value,contactTel:document.getElementById('f-contact-tel').value,contactEmail:document.getElementById('f-contact-email').value,categorie:selectedCat,formation:document.getElementById('f-formation').value,commentaire:document.getElementById('f-commentaire').value};
  const adesc=document.getElementById('f-action-desc').value;
  const adate=document.getElementById('f-action-date').value;
  if(id){
    const i=prospects.findIndex(p=>p.id===id);
    if(i!==-1){
      prospects[i]={...prospects[i],...data};
      if(adesc&&adate)prospects[i].prochaineAction={description:adesc,date:adate,hstart:document.getElementById('f-action-hstart').value,hend:document.getElementById('f-action-hend').value,assignee:document.getElementById('f-action-assignee').value,createdAt:new Date().toISOString()};
      showNotif('Prospect mis à jour');
    }
  }else{
    const np={id:generateId(),...data,actionsEffectuees:[],prochaineAction:null,createdAt:new Date().toISOString()};
    if(adesc&&adate)np.prochaineAction={description:adesc,date:adate,hstart:document.getElementById('f-action-hstart').value,hend:document.getElementById('f-action-hend').value,assignee:document.getElementById('f-action-assignee').value,createdAt:new Date().toISOString()};
    prospects.push(np);showNotif('Prospect créé');
  }
  saveData();closeProspectModal();
}
function editProspect(id){openProspectModal(id);}
function deleteProspect(id){
  if(confirm('Supprimer ce prospect ?')){
    prospects=prospects.filter(p=>p.id!==id);
    saveData();showNotif('Supprimé','warning');
    closeDV();
  }
}
function selectCat(c){
  selectedCat=c;
  document.querySelectorAll('.cat-opt').forEach(o=>o.classList.toggle('selected',o.dataset.v===c));
}

// ACTION MODAL
function openActionModal(pid=null,fromAgenda=false,preDate='',preHour=''){
  document.getElementById('action-form').reset();
  const ps=document.getElementById('action-prospect-search');
  const pidEl=document.getElementById('action-prospect-id');
  if(pid){
    const p=prospects.find(x=>x.id===pid);
    if(p){ps.value=p.nom;pidEl.value=pid;}
  }else{ps.value='';pidEl.value='';}
  if(preDate)document.getElementById('action-date').value=preDate;
  if(preHour)document.getElementById('action-hstart').value=preHour;
  document.getElementById('action-modal-title').textContent=fromAgenda?'Nouvelle tâche agenda':'Planifier une action';
  document.getElementById('action-modal').classList.add('open');
}
function closeActionModal(){document.getElementById('action-modal').classList.remove('open');document.getElementById('prospect-dropdown').style.display='none';}

function searchProspectInput(v){
  const dd=document.getElementById('prospect-dropdown');
  if(!v){dd.style.display='none';return;}
  const matches=prospects.filter(p=>p.nom.toLowerCase().includes(v.toLowerCase())).slice(0,8);
  if(!matches.length){dd.style.display='none';return;}
  dd.innerHTML=matches.map(p=>`<div onclick="selectProspect('${p.id}','${esc(p.nom)}')" style="padding:10px 14px;cursor:pointer;font-size:13.5px;border-bottom:1px solid var(--bl)" onmouseover="this.style.background='var(--bg3)'" onmouseout="this.style.background=''"><strong>${esc(p.nom)}</strong> <span style="color:var(--txt3);font-size:12px">${p.contactNom||''}</span></div>`).join('');
  dd.style.display='block';
}
function selectProspect(id,nom){
  document.getElementById('action-prospect-id').value=id;
  document.getElementById('action-prospect-search').value=nom;
  document.getElementById('prospect-dropdown').style.display='none';
}

function handleActionSubmit(e){
  e.preventDefault();
  const pid=document.getElementById('action-prospect-id').value;
  if(!pid){showNotif('Sélectionnez un prospect','error');return;}
  const desc=document.getElementById('action-desc').value;
  const date=document.getElementById('action-date').value;
  const action={description:desc,date:date,hstart:document.getElementById('action-hstart').value,hend:document.getElementById('action-hend').value,assignee:document.getElementById('action-assignee').value,notes:document.getElementById('action-notes').value,createdAt:new Date().toISOString()};
  const i=prospects.findIndex(p=>p.id===pid);
  if(i!==-1){
    // Archive old if exists
    if(prospects[i].prochaineAction){
      if(!prospects[i].actionsEffectuees)prospects[i].actionsEffectuees=[];
      prospects[i].actionsEffectuees.push({...prospects[i].prochaineAction,completedAt:new Date().toISOString()});
    }
    prospects[i].prochaineAction=action;
    saveData();showNotif('Action planifiée ✅');closeActionModal();
    if(document.getElementById('detail-view').classList.contains('open'))openDetailView(pid);
    if(currentView==='agenda')renderAgenda();
  }
}
function completeAction(pid){
  const i=prospects.findIndex(p=>p.id===pid);
  if(i!==-1&&prospects[i].prochaineAction){
    if(!prospects[i].actionsEffectuees)prospects[i].actionsEffectuees=[];
    prospects[i].actionsEffectuees.push({...prospects[i].prochaineAction,completedAt:new Date().toISOString()});
    prospects[i].prochaineAction=null;
    saveData();showNotif('Action effectuée ✓');
    if(document.getElementById('detail-view').classList.contains('open'))openDetailView(pid);
    if(currentView==='agenda')renderAgenda();
  }
}

// IMPORT / EXPORT
function openImportModal(){document.getElementById('import-modal').classList.add('open');}
function closeImportModal(){document.getElementById('import-modal').classList.remove('open');}
function handleFileSelect(event){
  const file=event.target.files[0];if(!file)return;
  const ext=file.name.split('.').pop().toLowerCase();
  if(ext==='csv'){Papa.parse(file,{header:true,delimiter:';',skipEmptyLines:true,complete:r=>importData(r.data),error:()=>showNotif('Erreur fichier','error')});}
  else if(ext==='xlsx'||ext==='xls'){const reader=new FileReader();reader.onload=function(e){const data=new Uint8Array(e.target.result);const wb=XLSX.read(data,{type:'array'});const ws=wb.Sheets[wb.SheetNames[0]];importData(XLSX.utils.sheet_to_json(ws));};reader.readAsArrayBuffer(file);}
  event.target.value='';
}
function importData(data){
  let n=0;
  data.forEach(row=>{
    const nom=row.nom_etablissement||row['nom_etablissement'];if(!nom)return;
    const np={id:generateId(),nom:nom,contactNom:row.contact_nom||'',contactTel:row.contact_telephone||'',contactEmail:row.contact_email||'',adresse:row.adresse||'',siret:row.siret||'',categorie:row.categorie||'chaud',formation:row.formation_visee||'',commentaire:row.commentaire||'',actionsEffectuees:[],prochaineAction:null,createdAt:new Date().toISOString()};
    const pd=row.prochaine_action;const pdate=row.date_prochaine_action;
    if(pd&&pdate)np.prochaineAction={description:pd,date:pdate,createdAt:new Date().toISOString()};
    prospects.push(np);n++;
  });
  saveData();closeImportModal();showNotif(`${n} prospect(s) importé(s)`);
}
function exportData(){
  const csv=Papa.unparse(prospects.map(p=>({nom_etablissement:p.nom,contact_nom:p.contactNom||'',contact_telephone:p.contactTel||'',contact_email:p.contactEmail||'',adresse:p.adresse||'',siret:p.siret||'',categorie:p.categorie,formation_visee:p.formation||'',commentaire:p.commentaire||'',prochaine_action:p.prochaineAction?.description||'',date_prochaine_action:p.prochaineAction?.date||'',actions_effectuees:p.actionsEffectuees?.map(a=>a.description).join(' | ')||''})),{delimiter:';'});
  const blob=new Blob(['\ufeff'+csv],{type:'text/csv;charset=utf-8'});
  const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download=`siphro-export-${new Date().toISOString().split('T')[0]}.csv`;a.click();URL.revokeObjectURL(url);
  showNotif('Export téléchargé');
}

// DRAG & DROP IMPORT
const iz=document.getElementById('import-zone');
iz.addEventListener('dragover',e=>{e.preventDefault();iz.classList.add('dragover');});
iz.addEventListener('dragleave',()=>iz.classList.remove('dragover'));
iz.addEventListener('drop',e=>{e.preventDefault();iz.classList.remove('dragover');const f=e.dataTransfer.files[0];if(f)handleFileSelect({target:{files:[f]},}); });

// NOTIF
function showNotif(msg,type='success'){
  const el=document.getElementById('notif');
  el.className='notif'+(type==='error'?' error':type==='warning'?' warning':'');
  document.getElementById('notif-ico').textContent=type==='error'?'✗':type==='warning'?'⚠':'✓';
  document.getElementById('notif-text').textContent=msg;
  el.style.display='flex';setTimeout(()=>el.style.display='none',3000);
}

// CLOSE DROPDOWN ON OUTSIDE CLICK
document.addEventListener('click',e=>{
  const dd=document.getElementById('prospect-dropdown');
  if(dd&&!dd.contains(e.target)&&e.target.id!=='action-prospect-search')dd.style.display='none';
});

// INIT
document.addEventListener('DOMContentLoaded',loadData);
</script>
</body>
</html>
