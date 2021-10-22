# test__
open



HG


select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 (
(select :chap1 as lib1,:descp1 as lib2, round(SUM(case when :chap1 =substr(lib2,1,3) || '%'then mnt1 else 0 END), 2) as mnt1, ' ' as lib3, :chap3 as lib4, :descp2 as lib5,round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:date, 'dd/MM/yyyy'))
UNION ALL
(select :chap2 as lib1,:descp3 as lib2, round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt1, ' ' as lib3, ' ' as lib4, ' ' as lib5, ' ' as mnt2, ' ' as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:date, 'dd/MM/yyyy') )
)  where mnt3 !=0




select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from (
(select :chap1 as lib1,:descp1 as lib2, round(SUM(case when :chap1 =substr(lib2,1,3) || '%'then mnt1 else 0 END), 2) as mnt1, ' ' as lib3, :chap3 as lib4, :descp2 as lib5,round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy'))
)  where mnt3 !=0;
UNION ALL
select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, '' as mnt3 from (
(select :chap2 as lib1,:descp3 as lib2, round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt1, ' ' as lib3, ' ' as lib4, ' ' as lib5, ' ' as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy') )
)  where mnt3 !=0;
select dop from scinv  GROUP BY dop order by dop desc;



(select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from (
(select :chap1 as lib1,:descp1 as lib2, round(SUM(case when :chap1 =substr(lib2,1,3) || '%'then mnt1 else 0 END), 2) as mnt1, ' ' as lib3, :chap3 as lib4, :descp2 as lib5,round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy'))
)  where mnt3 !=0)
UNION ALL
(select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, Null as mnt3 from (
(select :chap2 as lib1,:descp3 as lib2, round(SUM(case when :chap2 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt1, ' ' as lib3, ' ' as lib4, ' ' as lib5, Null as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '009' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy') )
)  where mnt3 !=0);





select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from ((select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from (
(select :chap1 as lib1,:descp1 as lib2, round(SUM(case when :chap1 =substr(lib2,1,3) || '%'then mnt1 else 0 END), 2) as mnt1, ' ' as lib3, :chap3 as lib4, :descp2 as lib5,round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy'))
)  where mnt3 !=0)
UNION ALL
(select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, Null as mnt3 from (
(select :chap2 as lib1,:descp3 as lib2, round(SUM(case when :chap2 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt1, ' ' as lib3, ' ' as lib4, ' ' as lib5, Null as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy') )
)  where mnt3 !=0)) where  EXISTS (
        SELECT
            1
        from (select round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy')) where mnt3 !=0
    );
