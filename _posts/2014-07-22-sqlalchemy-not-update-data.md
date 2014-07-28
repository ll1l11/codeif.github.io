---
layout: post
title: 解决sqlalchemy不能写数据(插入,更新)
---


根据具体环境替换掉mysql的链接和密码, databasename, 开始的测试代码如下, 运行正常

    # -*- coding: utf-8 -*-
    
    from sqlalchemy import create_engine, Column, Integer, String
    from sqlalchemy.orm import sessionmaker
    from sqlalchemy.ext.declarative import declarative_base
    
    db_path = "mysql+mysqldb://root:root@p3-host/test?charset=utf8"
    engine = create_engine(db_path)
    
    DB_Session = sessionmaker(bind=engine)
    db_session = DB_Session()
    
    Base = declarative_base()
    
    class User(Base):
        __tablename__ = 'user'
    
        id = Column(Integer, primary_key=True)
        name = Column(String(255))
    
    
    def init_db():
        Base.metadata.create_all(bind=engine)
    
    def init_data():
        for i in range(10):
            u = User(name=str(i))
            db_session.add(u)
        db_session.commit()
    
    def show_users():
        users = db_session.query(User).all()
        for user in users:
            print user.name
    
    if __name__ == '__main__':
        show_users()

如果我们在mosql在加一个字段, 改成如下:

