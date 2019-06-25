# Renderer
Renderer Using D2D

## Making Background Black In color
 1. Create MFC Application MDI/SDI.
 2. Add OnDraw() function to the view which you wants to make screen color black
 3. You have to include Header file(optional) and D2D Library.
 <pre><code>
  #include <d2d1.h>
  #include <eh.h>
  #pragma comment(lib, "d2d1")
  </code></pre>
  4. Create 2 varaibles in header file which you need while calling D2D function
  <pre><code>
  ID2D1Factory			*m_pD2DFactory;
	 ID2D1HwndRenderTarget	*m_pRenderTargetHwn;
  </code></pre>
  <pre><code>
  void CRenderingView::OnDraw(CDC* pDC)
{
	// TODO: Add your specialized code here and/or call the base class
	HRESULT hr = S_OK;
	m_pD2DFactory = NULL;
	
		hr = ::D2D1CreateFactory(
			D2D1_FACTORY_TYPE_SINGLE_THREADED   //D2D1_FACTORY_TYPE_SINGLE_THREADED  D2D1_FACTORY_TYPE_MULTI_THREADED
			, &m_pD2DFactory);

		if (S_OK != hr)
		{
			AfxMessageBox(_T("Error Direct2d init setting."));
		}
		HWND hwnd = this->GetSafeHwnd();
		D2D1_RENDER_TARGET_PROPERTIES rtProps = D2D1::RenderTargetProperties();
		rtProps.usage = D2D1_RENDER_TARGET_USAGE_GDI_COMPATIBLE;

		RECT rect;
		::GetClientRect(hwnd, &rect);
		D2D1_SIZE_U size = D2D1::Size<UINT>(rect.right, rect.bottom);

		hr = m_pD2DFactory->CreateHwndRenderTarget(
			rtProps,												//			D2D1::RenderTargetProperties(),
			D2D1::HwndRenderTargetProperties(hwnd, size),
			&m_pRenderTargetHwn);

		PAINTSTRUCT ps;
		CDC* cdc = BeginPaint(&ps);
		if (m_pRenderTargetHwn->CheckWindowState() & D2D1_WINDOW_STATE_OCCLUDED)
			return;
		m_pRenderTargetHwn->BeginDraw();

		m_pRenderTargetHwn->Clear(D2D1::ColorF(0.0F, 0.0F, 0.0F));//0.7F, 0.8F, 0.0F for changing different color

		m_pRenderTargetHwn->EndDraw();
		EndPaint(&ps);
}
</code></pre>
