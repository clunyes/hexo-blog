---
title: AIDL的梳理
date: 2017-02-20 18:06:36
tags:
---
android IPC 不得不提到AIDL，其余广播 bundle ContentProvider以及socket

其中又以AIDL最为关键（在android中就是这样）。

其中又有一个关键字Binder，在我理解看来Binder是一个桥梁。

按照android开发艺术探索的指引，生成IBookManager.java文件

    /*
     * This file is auto-generated.  DO NOT MODIFY.
     * Original file: E:\\workspace_study\\ZKTestProject\\app\\src\\main\\aidl\\com\\zhaokang\\zktestproject\\IBookManager.aidl
     */
    package com.zhaokang.zktestproject;
    
    public interface IBookManager extends android.os.IInterface {
        /**
         * Local-side IPC implementation stub class.
         */
        public static abstract class Stub extends android.os.Binder implements com.zhaokang.zktestproject.IBookManager {
            private static final java.lang.String DESCRIPTOR = "com.zhaokang.zktestproject.IBookManager";
    
            /**
             * Construct the stub at attach it to the interface.
             */
            public Stub() {
                this.attachInterface(this, DESCRIPTOR);
            }
    
            /**
             * Cast an IBinder object into an com.zhaokang.zktestproject.IBookManager interface,
             * generating a proxy if needed.
             */
            public static com.zhaokang.zktestproject.IBookManager asInterface(android.os.IBinder obj) {
                if ((obj == null)) {
                    return null;
                }
                android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
                if (((iin != null) && (iin instanceof com.zhaokang.zktestproject.IBookManager))) {
                    return ((com.zhaokang.zktestproject.IBookManager) iin);
                }
                return new com.zhaokang.zktestproject.IBookManager.Stub.Proxy(obj);
            }
    
            @Override
            public android.os.IBinder asBinder() {
                return this;
            }
    
            @Override
            public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException {
                switch (code) {
                    case INTERFACE_TRANSACTION: {
                        reply.writeString(DESCRIPTOR);
                        return true;
                    }
                    case TRANSACTION_getBookList: {
                        data.enforceInterface(DESCRIPTOR);
                        java.util.List<com.zhaokang.zktestproject.Book> _result = this.getBookList();
                        reply.writeNoException();
                        reply.writeTypedList(_result);
                        return true;
                    }
                    case TRANSACTION_addBook: {
                        data.enforceInterface(DESCRIPTOR);
                        com.zhaokang.zktestproject.Book _arg0;
                        if ((0 != data.readInt())) {
                            _arg0 = com.zhaokang.zktestproject.Book.CREATOR.createFromParcel(data);
                        } else {
                            _arg0 = null;
                        }
                        this.addBook(_arg0);
                        reply.writeNoException();
                        return true;
                    }
                }
                return super.onTransact(code, data, reply, flags);
            }
    
            private static class Proxy implements com.zhaokang.zktestproject.IBookManager {
                private android.os.IBinder mRemote;
    
                Proxy(android.os.IBinder remote) {
                    mRemote = remote;
                }
    
                @Override
                public android.os.IBinder asBinder() {
                    return mRemote;
                }
    
                public java.lang.String getInterfaceDescriptor() {
                    return DESCRIPTOR;
                }
    
                @Override
                public java.util.List<com.zhaokang.zktestproject.Book> getBookList() throws android.os.RemoteException {
                    android.os.Parcel _data = android.os.Parcel.obtain();
                    android.os.Parcel _reply = android.os.Parcel.obtain();
                    java.util.List<com.zhaokang.zktestproject.Book> _result;
                    try {
                        _data.writeInterfaceToken(DESCRIPTOR);
                        mRemote.transact(Stub.TRANSACTION_getBookList, _data, _reply, 0);
                        _reply.readException();
                        _result = _reply.createTypedArrayList(com.zhaokang.zktestproject.Book.CREATOR);
                    } finally {
                        _reply.recycle();
                        _data.recycle();
                    }
                    return _result;
                }
    
                @Override
                public void addBook(com.zhaokang.zktestproject.Book book) throws android.os.RemoteException {
                    android.os.Parcel _data = android.os.Parcel.obtain();
                    android.os.Parcel _reply = android.os.Parcel.obtain();
                    try {
                        _data.writeInterfaceToken(DESCRIPTOR);
                        if ((book != null)) {
                            _data.writeInt(1);
                            book.writeToParcel(_data, 0);
                        } else {
                            _data.writeInt(0);
                        }
                        mRemote.transact(Stub.TRANSACTION_addBook, _data, _reply, 0);
                        _reply.readException();
                    } finally {
                        _reply.recycle();
                        _data.recycle();
                    }
                }
            }
    
            static final int TRANSACTION_getBookList = (android.os.IBinder.FIRST_CALL_TRANSACTION + 0);
            static final int TRANSACTION_addBook = (android.os.IBinder.FIRST_CALL_TRANSACTION + 1);
        }
    
        public java.util.List<com.zhaokang.zktestproject.Book> getBookList() throws android.os.RemoteException;
    
        public void addBook(com.zhaokang.zktestproject.Book book) throws android.os.RemoteException;
    }
    
    
stub就是binder类

    DESCRIPTOR是binder类名
    
    asInterface(android.os.IBinder obj)将服务端binder转换成客户端需要的接口类型，如果是同进程，返回stub，如果跨进程返回
    proxy。
    
    asBinder 返回binder
    
    onTransact  public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) 
    该方法运行在服务端的线程池中。
    transact翻译来是处理 当客户端发出ipc请求，最终由该方法处理，根据code来判断调用哪个方法，从data中取出数据，用reply返回数据。
    如果该方法返回false，客户端的请求会失败。
    
    Proxy#getBookList java.util.List<com.zhaokang.zktestproject.Book> getBookList() 创建data，reply，和最后的返回list
    
     if ((book != null)) {
        _data.writeInt(1);
         book.writeToParcel(_data, 0);
        } else {
             _data.writeInt(0);
        }
        。。。
     }
    
    如果有参数，就放入data，随后调用服务端的onTransact，同时当前线程挂起，等到onTransact返回reply，随后返回reply的数据
    
    注意点
    
    1. 客户端发起请求后线程会挂起，为了防止影响ui线程，建议在子线程中发起请求。
    2. onTransact运行在线程池中，所以必须用同步的方式去执行。
    
    
    