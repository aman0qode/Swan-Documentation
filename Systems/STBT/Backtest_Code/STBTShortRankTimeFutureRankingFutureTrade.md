## STBTShortRankTimeFutureRankingFutureTrade

    #include <F&O BanList.afl>
    #include <F&O expiry selection.afl>

    bi = BarIndex();
    exitlastbar = LastValue(bi) == bi ; 
    addtime = 0;
    Lev = 1;
    f = 0.1; 
    Shortvp = 25; 
    Covervp = 25;
    Shortbuffer = 1000;
    stopbuffer = 500; 
    maxpos = Optimize("maxpos",10,5,20,5);

    SetOption("InitialEquity",10000000);
    SetOption("AccountMargin",100/lev);
    SetOption("MaxOpenPositions",maxpos);
    SetPositionSize(100/Maxpos*lev,spsPercentOfEquity);
    SetOption("Priceboundchecking",False);

    SetTradeDelays(0,0,0,0);

    Cases = Optimize("Cases",5,1,7,1);

    if(cases == 1){ranktime = 094500+addtime;}
    if(cases == 2){ranktime = 100000+addtime;}
    if(cases == 3){ranktime = 110000+addtime;}
    if(cases == 4){ranktime = 120000+addtime;}
    if(cases == 5){ranktime = 130000+addtime;}
    if(cases == 6){ranktime = 140000+addtime;}
    if(cases == 7){ranktime = 150000+addtime;}

    Shorttime = ranktime + Shortbuffer;

    stoptime = Shorttime + stopbuffer;

    Covercase = Optimize("Sellcase",3,1,5,1);

    if(Covercase == 1){Covertime = 091500+addtime;}
    if(Covercase == 2){Covertime = 092000+addtime;}
    if(Covercase == 3){Covertime = 092500+addtime;}
    if(Covercase == 4){Covertime = 093000+addtime;}
    if(Covercase == 5){Covertime = 093500+addtime;}
    if(Covercase == 6){Covertime = 094000+addtime;}
    if(Covercase == 7){Covertime = 094500+addtime;}
    if(Covercase == 8){Covertime = 095000+addtime;}
    if(Covercase == 9){Covertime = 095500+addtime;}
    if(Covercase == 10){Covertime = 100000+addtime;}

    TimeFrameSet(inDaily);
    NextDay = valuewhen(Day() != Ref(Day(),1),Ref(DateNum(),1));
    NextDay = DateTimeConvert(2,NextDay);
    CurrentDate = DateTimeConvert(2,DateNum());
    Difference = round(DateTimeDiff(NextDay,CurrentDate)/(3600*24));
    TimeFrameRestore();

    Difference = TimeFrameExpand(Difference,inDaily,expandFirst);

    CurrentBarOpen = TimeFrameGetPrice("O" , inDaily ,0);

    Dailyclose = TimeFrameGetPrice("C",inDaily,0);

    GapPer = Optimize("GapPer",5,3,6,1);

    Gap = ( ( Close / CurrentBarOpen  ) -1 ) * 100;
    EODGap = ((Dailyclose/Currentbaropen)-1)*100;

    Short = Overnight AND Difference <= 10 AND Gap <= -GapPer AND TimeNum() == ranktime AND exitlastbar == 0 AND excludeok == 0;

    dow = ValueWhen(Short,DayOfWeek());

    Cover = (((TimeNum() == Covertime) ) AND DayOfWeek() != dow ) OR exitlastbar;

    ShortPrice = Close;

    CoverPrice = Open;

    ///Limit Order Logic
    /*
    vol = ValueWhen(TimeNum()== ranktime,(ATR(Shortvp))); 

    Strig = Ref(C,-1) + (vol * f); 

    entrytimeOK = TimeNum() >= Shorttime AND TimeNum() < stoptime;  

    trigOK = H > strig; 

    Shortcond1 = (trigOK AND entrytimeOK) AND ValueWhen(TimeNum() == ranktime,Gap) <= -GapPer; 
    Shortcond2 = (TimeNum() >= stoptime) AND ValueWhen(TimeNum() == ranktime,Gap) <= -GapPer; 

    Short = (Shortcond1 OR Shortcond2) AND Difference <= 10 AND overnight AND exitlastbar == 0 AND excludeok == 0; 

    covervol = Ref(ATR(Covervp),-1);
    ctrig = Ref(C,-1) - (covervol * f);
    coverlimit = IIf(DateNum() == 1200313, TimeNum() == 102000+addtime, TimeNum()==covertime) AND L < ctrig ;

    Cover = coverlimit OR (IIf(DateNum() == 1200313, TimeNum() > 102000+addtime, TimeNum() > covertime) AND TimeNum() < ranktime) OR exitlastbar;

    ShortPrice = IIf( trigok , Max(Open,Strig),Open);
    CoverPrice =IIf( coverlimit , Min(Open,ctrig),Open);
    */
    PositionScore = IIf(Gap <= -gapper , gap, 0); 

    PortEquity = Foreign("~~~Equity","C");

    Filter = 1;
    AddColumn(PortEquity,"PortEquity",1.2);
